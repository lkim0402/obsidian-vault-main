> A open source framework for building GenAI applications
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
### Templates
- [[PromptTemplate]]
- [[ChatPromptTemplate]]
### Text Splitter
- [[RecursiveCharacterTextSplitter]]

Other Useful stuff
- [[LCEL (LangChain Expression Language)]]
- [[load_summarize_chain]]