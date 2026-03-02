- **Tool**
	- functions/services an LLM can utilize to extend its capabilities 
	- many built in to the package (math, search, sql, etc)
	- u can create ur own tools (this will be the main use case)
		- we will be building tools based on the OpenAPI spec
- **Routing**
	- LLMs should be able to select which tools they should use
# setup + basics
```python
import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

from langchain.agents import tool
```

- defining more structured input schema using `SearchInput`
	- the description of the input is what the LLM uses to make decisions on what the input should be
	- so it should be clear
```python
from pydantic import BaseModel, Field
class SearchInput(BaseModel):
    query: str = Field(description="Thing to search for")
```

- `@tool`
	- automatically **converts to a langchain tool** we can use!
	- decorator we can use on top of a function we define
	- here we use `args_schema=SearchInput` (made a while ago)
```python
@tool(args_schema=SearchInput)
def search(query:str) -> str:
	"""Search for weather online"""
	return "42f"

search.name # 'search'
search.description
# 'search(query: str) -> str - Search for the weather online.'
search.args   
# {'query': {'title': 'Query', 'description': 'Thing to search for', 'type': 'string'}}
search.run("sf") # '42'
```
- we can execute using `run`
# example - `get_current_temperature`, `search_wikipedia`

## `get_current_temperature`
```python
import requests
from pydantic import BaseModel, Field
import datetime

# Define the input schema
class OpenMeteoInput(BaseModel):
    latitude: float = Field(..., description="Latitude of the location to fetch weather data for")
    longitude: float = Field(..., description="Longitude of the location to fetch weather data for")

@tool(args_schema=OpenMeteoInput)
def get_current_temperature(latitude: float, longitude: float) -> dict:
    """Fetch current temperature for given coordinates."""
    
    BASE_URL = "https://api.open-meteo.com/v1/forecast"
    
    # Parameters for the request
    params = {
        'latitude': latitude,
        'longitude': longitude,
        'hourly': 'temperature_2m',
        'forecast_days': 1,
    }

    # Make the request
    response = requests.get(BASE_URL, params=params)
    
    if response.status_code == 200:
        results = response.json()
    else:
        raise Exception(f"API Request failed with status code: {response.status_code}")

    current_utc_time = datetime.datetime.utcnow()
    time_list = [datetime.datetime.fromisoformat(time_str.replace('Z', '+00:00')) for time_str in results['hourly']['time']]
    temperature_list = results['hourly']['temperature_2m']
    
    closest_time_index = min(range(len(time_list)), key=lambda i: abs(time_list[i] - current_utc_time))
    current_temperature = temperature_list[closest_time_index]
    
    return f'The current temperature is {current_temperature}°C'
```

- we can convert this tool to openai function
```python
from langchain.tools.render import format_tool_to_openai_function

format_tool_to_openai_function(get_current_temperature)
get_current_temperature({"latitude": 13, "longitude": 14})
```

## `search_wikipedia`
```python
import wikipedia
@tool
def search_wikipedia(query: str) -> str:
    """Run Wikipedia search and get page summaries."""
    page_titles = wikipedia.search(query)
    summaries = []
    for page_title in page_titles[: 3]:
        try:
            wiki_page =  wikipedia.page(title=page_title, auto_suggest=False)
            summaries.append(f"Page: {page_title}\nSummary: {wiki_page.summary}")
        except (
            self.wiki_client.exceptions.PageError,
            self.wiki_client.exceptions.DisambiguationError,
        ):
            pass
    if not summaries:
        return "No good Wikipedia Search Result was found"
    return "\n\n".join(summaries)
```

# OpenAPI specs to callable tools
- OpenAPI specs
	- a JSON or YAML file that fully describes a RESTful API, detailing its endpoints, required parameters, authentication methods, and expected responses
	- u can get them thru string of text
- Why `openapi_spec_to_openai_fn` is Needed
	- Manually writing function schemas and Python wrapper code for every single endpoint in a large API (like Jira, Stripe, or a custom enterprise backend) is very repetitive and prone to error
	- the function automates the process -> **translates spec to list of available langchain tools we can use!!**
```python
from langchain.chains.openai_functions.openapi import openapi_spec_to_openai_fn
from langchain.utilities.openapi import OpenAPISpec

text = """
{
  "openapi": "3.0.0",
  "info": {
    "version": "1.0.0",
    "title": "Swagger Petstore",
    "license": {
      "name": "MIT"
    }
  },
  "servers": [
    {
      "url": "http://petstore.swagger.io/v1"
    }
  ],
  "paths": {
    "/pets": {
      "get": {
	... (cut it coz its long)
"""

spec = OpenAPISpec.from_text(text)
pet_openai_functions, pet_callables = openapi_spec_to_openai_fn(spec)
```

```python
print(pet_openai_functions)
```
```
[{'name': 'listPets',
  'description': 'List all pets',
  'parameters': {'type': 'object',
   'properties': {'params': {'type': 'object',
     'properties': {'limit': {'type': 'integer',
       'maximum': 100.0,
       'schema_format': 'int32',
       'description': 'How many items to return at one time (max 100)'}},
     'required': []}}}},
 {'name': 'createPets',
  'description': 'Create a pet',
  'parameters': {'type': 'object', 'properties': {}}},
 {'name': 'showPetById',
  'description': 'Info for a specific pet',
  'parameters': {'type': 'object',
   'properties': {'path_params': {'type': 'object',
     'properties': {'petId': {'type': 'string',
       'description': 'The id of the pet to retrieve'}},
     'required': ['petId']}}}}]
```

```python
from langchain.chat_models import ChatOpenAI

model = ChatOpenAI(temperature=0).bind(functions=pet_openai_functions)
model.invoke("what are three pets names")
# AIMessage(content='', additional_kwargs={'function_call': {'name': 'listPets', 'arguments': '{"params":{"limit":3}}'}})
model.invoke("tell me about pet with id 42")
# AIMessage(content='', additional_kwargs={'function_call': {'name': 'showPetById', 'arguments': '{"path_params":{"petId":"42"}}'}})
```
# Routing
- let's use the examples (`get_current_temperature`, `search_wikipedia`) we made a while ago
```python
functions = [
    format_tool_to_openai_function(f) for f in [
        search_wikipedia, get_current_temperature
    ]
]

model = ChatOpenAI(temperature=0).bind(functions=functions)

from langchain.prompts import ChatPromptTemplate
prompt = ChatPromptTemplate.from_messages([
    ("system", "You are helpful but sassy assistant"),
    ("user", "{input}"),
])
chain = prompt | model


chain.invoke("what is the weather in sf right now")
# AIMessage(content='', additional_kwargs={'function_call': {'name': 'get_current_temperature', 'arguments': '{"latitude":37.7749,"longitude":-122.4194}'}})

chain.invoke("what is langchain")
# AIMessage(content='', additional_kwargs={'function_call': {'name': 'search_wikipedia', 'arguments': '{"query":"Langchain"}'}})
chain.invoke("hello")
# AIMessage(content='Hello! How can I assist you today?')
```
- this is good, but its annoying because the answer is wrapped in `AIMessage`
## using `OpenAIFunctionsAgentOutputParser()`
```python
from langchain.agents.output_parsers import OpenAIFunctionsAgentOutputParser

chain = prompt | model | OpenAIFunctionsAgentOutputParser()
result = chain.invoke({"input": "what is the weather in sf right now"})
type(result) # langchain.schema.agent.AgentActionMessageLog
result.tool # 'get_current_temperature'
result.tool_input # {'latitude': 37.7749, 'longitude': -122.4194}
```
- with the data of which function to call now, we want to use the data to call the function

```python
get_current_temperature(result.tool_input)
# 'The current temperature is 7.2°C'
```
## `AgentAction` & `AgentFinish`
if there is no tool to call, the result is `AgentFinish`
```python
result = chain.invoke({"input": "hi!"})
type(result) # AgentFinish
result.return_values # {'output': 'Hello! How can I assist you today?'}
```

- scenarios
	- if a function is called -> `AgentAction`
	- if a function is NOT called -> `AgentFinish`
## Route function
- acts on the result of the LLM -> executes the functions with corresponding data
```python
from langchain.schema.agent import AgentFinish
def route(result):
    if isinstance(result, AgentFinish):
        return result.return_values['output']
    else:
        tools = {
            "search_wikipedia": search_wikipedia, 
            "get_current_temperature": get_current_temperature,
        }
        return tools[result.tool].run(result.tool_input)
```

```python
chain = prompt | model | OpenAIFunctionsAgentOutputParser() | route
```