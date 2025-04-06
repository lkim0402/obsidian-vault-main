> The basic template for creating prompts. Used for general-purpose tasks where a string of text is formatted to serve as a prompt for the model.

- A prompt template consists of a string template. It accepts a set of parameters from the user that can be used to generate a prompt for a language model.

```python
#Method1
from langchain.prompts import PromptTemplate

promt = PromptTemplate.from_template("Tell me about {city}.")
prompt.format(city = "Yongin-si").
#Result: Tell me about Yongin-si.

#Method 2
prompt = PromptTemplate(input_variables=["city"], template="Tell me about {city}.")
prompt.format(city = "Yongin-si")
#Result: Tell me about Yongin-si.
```