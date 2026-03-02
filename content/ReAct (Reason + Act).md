- A ReAct agent is an AI agent that uses the "reasoning and acting" framework to combine chain of thought (CoT) reasoning and external tool use
	- *Reasoning + Acting*
	- The LLM first thinks about what to do and then decides an action to take. The action then is executed in an environment and an observation is returned. repeats
# Steps
1. Reasoning (Thought)
	- The agent's internal monologue
	- Instead of immediately guessing an answer, the LLM is prompted to explicitly output its thought process. It analyzes the current state, breaks down the problem, plans the next step, or evaluates whether a previous step failed and needs correction.
2. Acting (Action)
	- the agent interacts with the outside world. Based on its `Thought`, the agent selects a predefined tool from its arsenal and provides the necessary parameters
	- _Examples:_ `SearchWikipedia("Distributed Systems")`, `ExecuteSQL("SELECT * FROM users WHERE active = true")`, or `CallWeatherAPI("Seattle")`.
3. Observation
	- The environment **executes the tool and returns the result** (the `Observation`) to the LLM
	- The agent does not generate this part; it simply ingests the raw data (e.g., a JSON response, a database row, or an error code).
	- The agent then uses this Observation to generate its next `Thought`, continuing the cycle **until the objective is completed**
## Example
Here is how a ReAct agent processes a prompt like: _"What is the current status of the database server in the us-west-2 region?"_
- **Thought 1:** I need to find the status of the database server. I should first look up the specific instance ID for the database in us-west-2.
- **Action 1:** `QueryAWSInventory(region="us-west-2", resource_type="rds")`
- **Observation 1:** `[{"instance_id": "db-prod-01", "engine": "postgresql"}]`
- **Thought 2:** The instance ID is db-prod-01. Now I need to check its current health status using the monitoring tool.
- **Action 2:** `CheckServerHealth(instance_id="db-prod-01")`
- **Observation 2:** `{"status": "available", "cpu_utilization": "42%"}`
- **Thought 3:** I have the status and it is available. I can now answer the user's question.
- **Final Answer:** The database server (db-prod-01) in us-west-2 is currently available with a CPU utilization of 42%.
# Building an agent from scratch

- a basic agent is not that hard to build
	- we will distinguish what jobs fall to the LLM or externally (the runtime, around the LLM)
- we will build this around [[ReAct (Reason + Act)]]
	- Reasoning + acting

## setup
```python
import openai
import re
import httpx
import os
from dotenv import load_dotenv

_ = load_dotenv()
from openai import OpenAI

client = OpenAI()
```

## `Agent` class
- [[Classes in Python]]
```python
class Agent:
	def __init__(self, system=""):
		self.system = system
		self.messages = []
		if self.system:
			self.messages.append({"role":"system","content":system})
	
	def __call__(self, message):
		self.messages.append({"role":"user", "content":message})
	    result = self.execute()
	    self.messages.append({"role":"assistant","content":result})
	    return result
	
	# helper for __call__
	def execute(self):
        completion = client.chat.completions.create(
                        model="gpt-4o", 
                        temperature=0,
                        messages=self.messages)
        return completion.choices[0].message.content
```
- `Agent` class
	- parameterized by a system message, allowing the user to pass that in 
- `def __call__(self, message):`
	- takes the string message and append to existing message list -> call the AI with it -> add the AI's response to message list

## The prompt we will use
```python
prompt = """
You run in a loop of Thought, Action, PAUSE, Observation.
At the end of the loop you output an Answer
Use Thought to describe your thoughts about the question you have been asked.
Use Action to run one of the actions available to you - then return PAUSE.
Observation will be the result of running those actions.

Your available actions are:

calculate:
e.g. calculate: 4 * 7 / 3
Runs a calculation and returns the number - uses Python so be sure to use floating point syntax if necessary

average_dog_weight:
e.g. average_dog_weight: Collie
returns average weight of a dog when given the breed

Example session:

Question: How much does a Bulldog weigh?
Thought: I should look the dogs weight using average_dog_weight
Action: average_dog_weight: Bulldog
PAUSE

You will be called again with this:

Observation: A Bulldog weights 51 lbs

You then output:

Answer: A bulldog weights 51 lbs
""".strip()
```
- we instruct it to be `ReAct` -> `Thought`, `Action`, `PAUSE`, `Observation`. it can then output an answer only when it finishes that loop
	- `Thought`
		- describe its thoughts about the question its asked
	- `Action`
		- run one of the actions available to it 
		- included in the prompt
	- `PAUSE`
	- `Observation`
		- used to signal the result o running those actions
- we add an example too 
## tools
```python
def calculate(what):
    return eval(what)

def average_dog_weight(name):
    if name in "Scottish Terrier": 
        return("Scottish Terriers average 20 lbs")
    elif name in "Border Collie":
        return("a Border Collies average weight is 37 lbs")
    elif name in "Toy Poodle":
        return("a toy poodles average weight is 7 lbs")
    else:
        return("An average dog weights 50 lbs")

# dictionary
known_actions = {
    "calculate": calculate,
    "average_dog_weight": average_dog_weight
}
```
- these are just toy examples
## making ai agent manually
- we're just gonna manually add the observation (result of the action)

```python
abot = Agent(prompt)
result = abot("How much does a toy poodle weigh?")
print(result)
```
```
Thought: I should look up the average weight of a Toy Poodle using the average_dog_weight action.
Action: average_dog_weight: Toy Poodle
PAUSE
```
- There is a `Thought`, `Action` and `PAUSE`
- it means that the AI thinks that we should look up `Toy Poodle` using `average_dog_weight`

```python
result = average_dog_weight("Toy Poodle") 
# 'a toy poodles average weight is 7 lbs'
```

- we format to the next prompt for `Observation`
```python
next_prompt = "Observation: {}".format(result)
abot(next_prompt)
# 'Answer: A Toy Poodle weighs an average of 7 lbs.'
```

- we can see the `abot.messages` (accumulation)
	- ![[react-ai-msg.png]]
- another example
	- ![[Pasted image 20260220130943.png]]
	- 

## automating the loop!
```python
action_re = re.compile('^Action: (\w+): (.*)$')   
# python regular expression to selection action
```
- parse the response
	- look for the action string -> determine if we want to take an action or if its a final answer.

- automated code of what we did a while ago
```python
known_actions = {
    "calculate": calculate,
    "average_dog_weight": average_dog_weight
}

def query(question, max_turns=5):
	i = 0
	bot = Agent(prompt)
	next_prompt = question
	
	while i < max_turns:
		i += 1
		# call agent and get result back
		result = bot(next_prompt)
		print(result)
		# use regex to parse the response, get back a list of actions
		actions = [
			action_re.match(a)
            for a in result.split('\n') 
            if action_re.match(a)
		]
		
		# if there is an action to run
		if actions:
			action, action_input = actions[0].groups()
		
            if action not in known_actions:
                raise Exception("Unknown action: {}: {}".format(action, action_input))
                
            print(" -- running {} {}".format(action, action_input))
            
            observation = known_actions[action](action_input)
            print("Observation:", observation)
            
            next_prompt = "Observation: {}".format(observation)
		else:
			return

```