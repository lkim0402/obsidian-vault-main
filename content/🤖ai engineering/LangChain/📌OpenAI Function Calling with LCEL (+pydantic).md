- We use [[Pydantic]] to create schemas & create the openai functions descriptions
	- [[OpenAI Function Calling - general idea]]
	- we've used just big blobs of json

# setup
```python
import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

from typing import List
from pydantic import BaseModel, Field
```
# pydantic & openai function definition
- [[Pydantic#Syntax|Pydantic syntax]]
- [[OpenAI Function Calling - general idea]]
	- rn we're just repeating what we did previously (making the functions for openai) but just with pydantic -> just creating the schema
```python
class WeatherSearch(BaseModel):
    """Call this with an airport code to get the weather at that airport"""
    airport_code: str = Field(description="airport code to get weather for")
    
from langchain.utils.openai_functions import convert_pydantic_to_openai_function
weather_function = convert_pydantic_to_openai_function(WeatherSearch)
```
```
{'name': 'WeatherSearch',
 'description': 'Call this with an airport code to get the weather at that airport',
 'parameters': {'title': 'WeatherSearch',
  'description': 'Call this with an airport code to get the weather at that airport',
  'type': 'object',
  'properties': {'airport_code': {'title': 'Airport Code',
    'description': 'airport code to get weather for',
    'type': 'string'}},
  'required': ['airport_code']}}
```

- you need a function description string `""""""` or else u cannot convert
	- only the function description is mandatory 

# with [[LCEL (LangChain Expression Language)|LCEL]]
```python
from langchain_chat_models import ChatOpenAI
model = ChatOpenAI()
```
## diff ways
- we can pass in the functions as arguments
```python
model.invoke("what is the weather in SF today?", functions=[weather_function])
```
```
AIMessage(content='', additional_kwargs={'function_call': {'name': 'WeatherSearch', 'arguments': '{"airport_code":"SFO"}'}})
```

- we can bind the function invocation to the model
	- [[LCEL (LangChain Expression Language)#Bind]]
	- u also have to reassign to a new variable because `Runnable`s are immutable
```python
model = model.bind(functions=[weather_function])
model.invoke("what is the weather in SF today?")
```
```
AIMessage(content='', additional_kwargs={'function_call': {'name': 'WeatherSearch', 'arguments': '{"airport_code":"SFO"}'}})
```
## we can still force the model to use a function
- [[OpenAI Function Calling - general idea#function call options|OpenAI Function Calling - general idea -> function call options]]
	- `auto`, `none`, specific function (ex. `function_call={"name": "get_current_weather"}`)
```python
model_with_forced_function = model.bind(
	functions=[weather_function], 
	function_call={"name":"WeatherSearch"}
)
model_with_forced_function.invoke("hi!")
```
```
AIMessage(content='', additional_kwargs={'function_call': {'name': 'WeatherSearch', 'arguments': '{"airport_code":"SFO"}'}})
```
- still calls the weather search 
## in a chain
```python
from langchain.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant"),
    ("user", "{input}")
])
chain = prompt | model_with_function
chain.invoke({"input": "what is the weather in sf?"})
```
## multiple functions
- Even better, we can pass a set of function and let the LLM decide which to use based on the question context
```python
class ArtistSearch(BaseModel):
    """Call this to get the names of songs by a particular artist"""
    artist_name: str = Field(description="name of artist to look up")
    n: int = Field(description="number of results")
class WeatherSearch(BaseModel):
    """Call this with an airport code to get the weather at that airport"""
    airport_code: str = Field(description="airport code to get weather for")
functions = [
    convert_pydantic_to_openai_function(WeatherSearch),
    convert_pydantic_to_openai_function(ArtistSearch),
]
model_with_functions = model.bind(functions=functions)
model_with_functions.invoke("what is the weather in sf?")
# AIMessage(content='', additional_kwargs={'function_call': {'name': 'WeatherSearch', 'arguments': '{"airport_code":"SFO"}'}})
model_with_functions.invoke("what are three songs by taylor swift?")
# AIMessage(content='', additional_kwargs={'function_call': {'name': 'ArtistSearch', 'arguments': '{"artist_name":"Taylor Swift","n":3}'}})
model_with_functions.invoke("hi!")
# AIMessage(content='Hello! How can I assist you today?')
```