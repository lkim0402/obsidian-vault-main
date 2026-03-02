- [[📌Tools and Routing]], chat memory -> chatgpt
- [[LCEL (LangChain Expression Language)]]
# Agent basics
- Combination of LLMs and code
	- LLMs - reasons about what steps to take and calls for actions (inputs to actions)
- agent loop
	- choose a tool to use
	- observe the output of the tool
	- repeat until a stopping condition is met
- stopping conditions can be
	- LLM determined - using LLM to determine when to stop (like `AgentFinish`)
	- Hard coded rules (like max num of iterations)
- *what we will do*
	- write own agent loop using [[LCEL (LangChain Expression Language)|LCEL]]|
	- utilize `agent_executor` which
		- implements the agent loop
		- adds error handling, early stopping, tracing, etc
# setup
```python
import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']
```

- we get the demo tools from [[📌Tools and Routing#example - `get_current_temperature`, `search_wikipedia`]]
```python
from langchain.tools import tool

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
    
tools = [get_current_temperature, search_wikipedia]
```

```python
from langchain.chat_models import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
from langchain.tools.render import format_tool_to_openai_function
from langchain.agents.output_parsers import OpenAIFunctionsAgentOutputParser

functions = [format_tool_to_openai_function(f) for f in tools]
model = ChatOpenAI(temperature=0).bind(functions=functions)
prompt = ChatPromptTemplate.from_messages([
    ("system", "You are helpful but sassy assistant"),
    ("user", "{input}"),
])
chain = prompt | model | OpenAIFunctionsAgentOutputParser()

result = chain.invoke({"input": "what is the weather is sf?"})
result.tool # 'get_current_temperature'
result.tool_input # {'latitude': 37.7749, 'longitude': -122.4194}
```
# creating agent loop manually

## the general idea
- An agent operates in a cycle: it determines which tool to use (**Action**), executes the tool to get data (**Observation**), and passes that data back to itself. It repeats this until it has enough information to answer the prompt
- To make this work, the LLM requires a short-term memory of the tools it just called and the results it received -> `agent_scratchpad`
```python
from langchain.prompts import MessagesPlaceholder

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are helpful but sassy assistant"),
    ("user", "{input}"),
    MessagesPlaceholder(variable_name="agent_scratchpad")
])
chain = prompt | model | OpenAIFunctionsAgentOutputParser()
```
- `MessagesPlaceholder`
	- list of messages
	- `agent_scratchpad` -> after passing `system, user` messages, we add the action + observation pairs
```python
result1 = chain.invoke({
    "input": "what is the weather is sf?",
    "agent_scratchpad": []
})
observation = get_current_temperature(result1.tool_input) #'The current temperature is 7.2°C'
```

```python
from langchain.agents.format_scratchpad import format_to_openai_functions

result1.message_log
scratchpad_history = format_to_openai_functions([(result1, observation), ])
# list of tuples
```

```
[AIMessage(content='', additional_kwargs={'function_call': {'name': 'get_current_temperature', 'arguments': '{"latitude":37.7749,"longitude":-122.4194}'}}),
 FunctionMessage(content='The current temperature is 7.2°C', name='get_current_temperature')]
```

```python
result2 = chain.invoke({
    "input": "what is the weather is sf?", 
    "agent_scratchpad": scratchpad_history
})
```
```
AgentFinish(return_values={'output': 'The current temperature in San Francisco is 7.2°C.'}, log='The current temperature in San Francisco is 7.2°C.')
```
## putting everything together
```python
from langchain.schema.runnable import RunnablePassthrough
from langchain.schema.agent import AgentFinish

# 1. Define the base chain (expects 'input' and 'agent_scratchpad')
chain = prompt | model | OpenAIFunctionsAgentOutputParser()
# chain.invoke({
#    "input": "what is the weather is sf?", 
#    "agent_scratchpad": []
#})

# 2. Define the translation layer (expects 'input' and 'intermediate_steps')
agent_chain = RunnablePassthrough.assign(
    agent_scratchpad = lambda x: format_to_openai_functions(x["intermediate_steps"])
) | chain

# 3. The Execution Loop
def run_agent(user_input):
    intermediate_steps = []
    
    while True:
        # Pass the raw history to agent_chain, which will format it and pass it to chain
        result = agent_chain.invoke({
            "input": user_input, 
            "intermediate_steps": intermediate_steps 
        })
        
        # Check if the LLM output a final conversational response
        if isinstance(result, AgentFinish):
            return result.return_values["output"]
        
        # If not AgentFinish, it is an AgentAction. Route to the correct tool.
        tool_func = {
            "search_wikipedia": search_wikipedia, 
            "get_current_temperature": get_current_temperature,
        }[result.tool]
        
        # Execute the tool
        observation = tool_func(result.tool_input)
        
        # Append the action and observation to the raw history
        intermediate_steps.append((result, observation))

# Example execution:
# print(run_agent("what is the weather in sf?"))
```

- basically when u `run_agent("what is the weather in sf?")` with `input` and `intermediate_steps`
	- `agent_chain` gets them, makes the `agent_scratchpad` based on `intermediate_steps`, passes `input` and `agent_scratchpad` to `chain`
# creating agent loop - `AgentExecutor` 
- basically what we did but better error handling and better logging and more functionality lol
```python
from langchain.agents import AgentExecutor

# Base chain (expects 'input' and 'agent_scratchpad')
chain = prompt | model | OpenAIFunctionsAgentOutputParser()
# chain.invoke({
#    "input": "what is the weather is sf?", 
#    "agent_scratchpad": []
#})

# 2Translation layer (expects 'input' and 'intermediate_steps')
agent_chain = RunnablePassthrough.assign(
    agent_scratchpad = lambda x: format_to_openai_functions(x["intermediate_steps"])
) | chain

agent_executor = AgentExecutor(agent=agent_chain, tools=tools, verbose=True)

agent_executor.invoke({"input": "what is langchain?"})
```
- but this doesn't hold memory

```python
prompt = ChatPromptTemplate.from_messages([
    ("system", "You are helpful but sassy assistant"),
    MessagesPlaceholder(variable_name="chat_history"),
    ("user", "{input}"),
    MessagesPlaceholder(variable_name="agent_scratchpad")
])
agent_chain = RunnablePassthrough.assign(
    agent_scratchpad= lambda x: format_to_openai_functions(x["intermediate_steps"])
) | prompt | model | OpenAIFunctionsAgentOutputParser()

# simple object - keeps list of messages in memory
from langchain.memory import ConversationBufferMemory
memory = ConversationBufferMemory(return_messages=True,memory_key="chat_history")

agent_executor = AgentExecutor(agent=agent_chain, tools=tools, verbose=True, memory=memory)
agent_executor.invoke({"input": "my name is bob"})
```
# making a chatbot (+ some advanced features)
```python
@tool
def create_your_own(query: str) -> str:
    """This function can do whatever you would like once you fill it in """
    print(type(query))
    return query[::-1]
tools = [get_current_temperature, search_wikipedia, create_your_own]
```

```python
import panel as pn  # GUI
pn.extension()
import panel as pn
import param

class cbfs(param.Parameterized):
    
    def __init__(self, tools, **params):
        super(cbfs, self).__init__( **params)
        self.panels = []
        self.functions = [format_tool_to_openai_function(f) for f in tools]
        self.model = ChatOpenAI(temperature=0).bind(functions=self.functions)
        self.memory = ConversationBufferMemory(return_messages=True,memory_key="chat_history")
        self.prompt = ChatPromptTemplate.from_messages([
            ("system", "You are helpful but sassy assistant"),
            MessagesPlaceholder(variable_name="chat_history"),
            ("user", "{input}"),
            MessagesPlaceholder(variable_name="agent_scratchpad")
        ])
        self.chain = RunnablePassthrough.assign(
            agent_scratchpad = lambda x: format_to_openai_functions(x["intermediate_steps"])
        ) | self.prompt | self.model | OpenAIFunctionsAgentOutputParser()
        self.qa = AgentExecutor(agent=self.chain, tools=tools, verbose=False, memory=self.memory)
    
    def convchain(self, query):
        if not query:
            return
        inp.value = ''
        result = self.qa.invoke({"input": query})
        self.answer = result['output'] 
        self.panels.extend([
            pn.Row('User:', pn.pane.Markdown(query, width=450)),
            pn.Row('ChatBot:', pn.pane.Markdown(self.answer, width=450, styles={'background-color': '#F6F6F6'}))
        ])
        return pn.WidgetBox(*self.panels, scroll=True)


    def clr_history(self,count=0):
        self.chat_history = []
        return 
```

```python
cb = cbfs(tools)

inp = pn.widgets.TextInput( placeholder='Enter text here…')

conversation = pn.bind(cb.convchain, inp) 

tab1 = pn.Column(
    pn.Row(inp),
    pn.layout.Divider(),
    pn.panel(conversation,  loading_indicator=True, height=400),
    pn.layout.Divider(),
)

dashboard = pn.Column(
    pn.Row(pn.pane.Markdown('# QnA_Bot')),
    pn.Tabs(('Conversation', tab1))
)
dashboard
```