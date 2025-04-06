> A specialized template designed for chat or conversational contexts. Typically structures prompts to stimulate a dialogue or interactive session.

`ChatPromptTemplate.from_messages`
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
"""
output:
	system: You are ChatGPT, a helpful assistant.
	user: Tell me a joke.
"""

```
