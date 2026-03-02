
>[!ChatPromptTemplate]
>A specialized template designed for chat or conversational contexts. Typically structures prompts to stimulate a dialogue or interactive session.

#  `ChatPromptTemplate.from_template`
```python
from langchain.chat_models import ChatOpenAI 
from langchain.prompts import ChatPromptTemplate
from langchain.schema.output_parser import StrOutputParser
#Initializing model
model = ChatOpenAI()

#Defining output parser to interpret model's response as a strong
output_parser = StrOutputParser()

#Defining a prompt template for generating a product description from a string of product notes
prompt = ChatPromptTemplate.from_template(
	"Create a lively and engaging product description with emojis based on these notes: \n" "{product_notes}"						  
)

chain = prompt | model | output_parser

# We can call invoke on the chain itself!
product_desc = chain.invoke({"product_notes": "Multi color affordable mobile covers"})

print(product_desc)
#Output: 🌈📱 Introducing our vibrant and pocket-friendly mobile covers! 🎉🤩 🌈🌟 Get ready to add a pop of color to your beloved device with our multi-colored mobile covers! Designed to make heads turn and hearts skip a beat, these cases are a true fashion statement in themselves. 🌈💃
```
# `ChatPromptTemplate.from_messages`
- used to create a **chat prompt template** from a list of message tuples. Each tuple contains a role (such as "system" or "user") and a corresponding message or template
- allows for the creation of dynamic chat prompts where parts of the message can be filled in with user-provided input or other variables

 Example
```python
from langchain.prompts import ChatPromptTemplate

# Define the system prompt
system_prompt = "You are ChatGPT, a helpful assistant."

# Create the chat prompt template
chat_prompt = ChatPromptTemplate.from_messages([
    ("system", system_prompt),
    ("user", "{input}")
])

# Example of how to use the template with user input
user_input = "Tell me a joke."
filled_prompt = chat_prompt.format(input=user_input)

print(filled_prompt)
# output
# system: You are ChatGPT, a helpful assistant.
# user: Tell me a joke.
```
