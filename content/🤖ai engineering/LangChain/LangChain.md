
>[!Langchain]
 An open source framework for building GenAI/LLM applications

 - Python and Javascript packages
 - Focused on composition and modularity
 - Key value adds
	 - modular components
	 - Use cases: common ways to combine components
- diagram
	![[Pasted image 20240813161021.png |600]]
```bash
pipenv install langchain
pipenv install langchain-openai
pipenv install langchain-community
pipenv install langchainhub
```
- `langchain-openai`: The 3rd party package which integrates langchain with OpenAI's LLMs
- `langchain-community`: The package that holds a lot of community contributed code like text splitters and output parsers
- `langchainhub`: Extension of LangChain, helps us to download prompts dynamically contributed by the community
- They are separate so we don't have to download bloat

runnables
- Each runnable is a self-contained unit that performs a specific task
- You can chain these units together (`|` operator) to define a workflow
	- `RunnableParallel`
		- executes multiple `Runnable` operations in parallel and aggregates their outputs into a single dictionary
		- used when needed to perform independent tasks concurrently
	- `RunnablePassthrough`
		- simply passes the input it receives unchanged to the next step in the chain
# Components
- [[Models, prompts, parsers]]
	- Models
		- LLMs: 20+ integrations
		- Chat models
		- Text embedding models: 10+ integrations
	- Prompts
		- [[PromptTemplate]]
		- [[ChatPromptTemplate]]
		- Output parsers: 5+ implementations
			- Retry/fixing logic
		- Example selectors: 5+ implementations
- [[LCEL (LangChain Expression Language)]]
	- [[Chains in langchain (outdated now)]]
- With OpenAI
	- [[OpenAI Function Calling - general idea]]
	- [[📌OpenAI Function Calling with LCEL (+pydantic)]]
		- [[Pydantic]]
- [[📌Tagging and Extraction]]
- [[📌Tools and Routing]]
- [[📌Conversational Agent]]
# Text Splitter

# Other Useful stuff
- [[load_summarize_chain]]