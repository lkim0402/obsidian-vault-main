All components have an `interface`, which includes:
- `stream`: stream back chunks of the response
- `invoke`: call the chain on an input
	- Called on `ChatPromptTemplate` object: takes in the input variables and substitutes the placeholders, creating a prompt that can be sent to a language model
	- Called on model: used as an input query to trigger a response
	- Called on output_parser: used to process the raw output from a language model and convert it into a structured format
- `batch`: call the chain on a list of inputs

### Normally this is how you would do it:
```python
  
!pip install langchain 
!pip install openai 
!pip install chromadb 
!pip install tiktoken

import os 
# Set the OPENAI_API_KEY environment variable 
os.environ['OPENAI_API_KEY'] = "..."

  
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

prompt_value = prompt.invoke({"product_notes": "Multi color affordable mobile covers"})
# Output: ChatPromptValue(messages=[HumanMessage(content='Create a lively and engaging product description with emojis based on these notes: \nMulti color affordable mobile covers')])

prompt_value.to_string()
#Output:'Human: Create a lively and engaging product description with emojis based on these notes: \nMulti color affordable mobile covers'

prompt_value.to_messages()
#Output:[HumanMessage(content='Create a lively and engaging product description with emojis based on these notes: \nMulti color affordable mobile covers')]

model_output = model.invoke(prompt_value.to_messages())
#Output: AIMessage(content="🌈📱Get ready to dress up your phone in a kaleidoscope of colors with our multi-color affordable mobile covers! 🎉💃✨🌈...(cut)"

output_parser.invoke(model_output)
#Output: 🌈📱Get ready to dress up your phone in a kaleidoscope of colors with our multi-color affordable mobile covers! 🎉💃✨🌈...(cut)"
```

### But using LCEL it's way easier:
```python
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

for chunk in chain.stream({"product_notes": "Multi color affordable mobile covers"}): print(chunk, end="", flush=True)
#Output; 🌈📱 Introducing our fabulous multi-color mobile covers! 🎉💃 Add a pop of color and a dash of personality to your phone with these affordable and trendy accessories! 💥💯

```
- You can even run in batches very easily, it handles parallelism code for you!
- https://python.langchain.com/v0.2/docs/introduction/

### LCEL with [[Retrieval Augmented Generation (RAG)]]
```python
from langchain.embeddings import OpenAIEmbeddings 
from langchain.prompts import ChatPromptTemplate 
from langchain.schema.runnable import RunnableParallel, RunnablePassthrough 
from langchain.vectorstores import Chroma


#External docs
docs = [ "Social Media and Politics: The Cambridge Analytica scandal revealed how social media data could be used to influence political opinions. The company collected personal data from millions of Facebook users without consent and used it for political advertising. This event sparked a global discussion on data privacy and the ethical responsibilities of social media platforms.", "Renewable Energy Progress: Solar power technology has seen significant advancements, with the development of photovoltaic cells that can convert more than 22% of sunlight into electricity. Countries like Germany and China are leading in solar energy production, contributing to a global shift towards renewable energy sources to combat climate change and reduce reliance on fossil fuels." ]

#vectorstore = embedding database
vectorstore = Chroma.from_texts( 
	docs, 
	embedding=OpenAIEmbeddings(), 
) 
# Retriever: a tool that searches an embedding database created from the documents provided in the `docs` variable.
retriever = vectorstore.as_retriever(search_kwargs={"k": 1}) 
retriever.invoke("how social media data used to influence political opinions")
#Output: [Document(page_content='Social Media and Politics: The Cambridge Analytica scandal revealed how social media data could be used to influence political opinions. The company collected personal data from millions of Facebook users without consent and used it for political advertising. This event sparked a global discussion on data privacy and the ethical responsibilities of social media platforms.')]

template = """Answer the question based only on the following context: {context} Question: {question} """ 
prompt = ChatPromptTemplate.from_template(template) 

model = ChatOpenAI() 
output_parser = StrOutputParser() 

# setup_and_retrieval = RunnableParallel( 
#   {"context": retriever, # `context` is processed by the `retriever`.
#   "question": RunnablePassthrough() # `question` is processed by `RunnablePassthrough`
#  } 
# ) 

chain = {"context": retriever, "question": RunnablePassthrough()} | prompt | model | output_parser 

chain.invoke("how social media data used to influence political opinions")
```
- Chain Process
	- The `retriever` searches external docs and retrieves relevant chunks related to query
	- `RunnablePassThrough()` simply forwards the query
	- The retrieved context (relevant document chunks) and the original question are combined into a structured format by the `prompt`
	- The `prompt` template generates a complete prompt using the combined context and question
	- The `model` generates a response based on the prompt
    - The `output_parser` processes and formats the response.