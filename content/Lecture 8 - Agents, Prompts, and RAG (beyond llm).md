- https://youtu.be/k1njvbBmfsw?si=Wo1w43E32ObIJdF4

# Augmenting LLMS
- *Why do we need to augment LLMs?*
	- It's very hard to control LLMs
	- LLM may underperform in your task
		- Domain-specific knowledge gaps
			- They lack domain knowledge, current knowledge, very specific narrow knowledge. U want the LLMs to know them without training them from scratch
			- ex) medical diagnosis
		- Inconsistencies in style or format
			- ex) legal writing
		- Task specific understanding
			- ex) classification in a niche field
		- Limited context handling
			- ex) summarizing long documents
	- Context windows are limited
		- u have to chunk it, embed it etc
		- Attention mechanism -> not good in attending very large context. They struggle to remember information in large contexts (Needle in a haystack).
- *Two dimensions to enhance LLMs*
	- Model + context optimization
		![[Pasted image 20260210201735.png]]
	- model - we improve the foundation model itself (`gpt 3.5` -> `gpt 5`)
	- context optimization - we can actually leverage this better
		- things we will learn today
# Prompt Engineering
- Why does this matter?
- Types of prompters
	- centaurs
		- those who dive and delegate their solution creation activities to the AI or to themselves
		- might give a big task (write long prompt, sends prompt, waits, and return for output)
	- cyborgs
		- those who completely integrate their task flow with the AI and continually interact with the tech
		- not delegate whole tasks, works quickly with the model back and forth
- *Basic Prompt Design Principles*
	- Ideas: Add examples of a good output, let it roleplay as an "expert", critique, chain of thoughts
	- Outline
		![[Pasted image 20260210202520.png]]
- Look at other ppl's prompts
	- Prompt repos on github (awesome prompt template). 
- Some strategies
	- few shot prompting
	- chaining (not chain of thought)
- *Zero shot vs few shot prompting*
	- zero shot
		- model is asked to perform the task without any examples or prior context
		- Ex) Classify the tone of the sentence as positive, negative, or neutral: The product is fine, but I was expecting more.
	- few-shot prompt
		- model is given examples of outputs before being asked to generate a new one (helps style and structure)
		- Ex) Classify the tone of the sentence as positive, negative, or neutral: The product is fine, but I was expecting more. Here are examples of tone classification: "..." -> Negative. "..." -> Positive. "..." -> Neutral. Now classify this sentence "The product is fine, but I was expecting more."
- *Chaining*
	- You chain complex prompts to improve performance
	- Example
		![[Pasted image 20260210203519.png]]
	- the final result is better, but also we can test those 3 prompts separately from each other. It helps you control your workflow and debug it more seamlessly.
# Fine-Tuning
- not a fan (there's limitations)
	- requires substantial labeled data for the fine-tuning task
	- fine-tuned models may overfit to specific data, losing general -purpose utility
	- time and cost-sensitive, especially if the base model frequently updates
- when to use
	- if the task requires repeated, high-precision outputs (ex. legal writing, scientific explanations)
	- if the general purpose llm consistently struggles with domain specific language
# Retrieval-Augmented Generation (RAG)
- VERY common interview question
- Solves the challenges with standalone LLMs (mentioned in augmenting LLMs)
	- Integrates external knowledge sources (ex. databases, documents, APIs)
	- Ensures answers are more accurate, up to date, and grounded
	- More developer control. Allows for targeted customization without retraining the model
- Question Answering with RAG example
	- diagram (simple vanilla rag)
		![[rag_example.png]]
	- The knowledge base
		- U use LLM embeddings to embed those documents to lower dimensional representations. If the representation is too small, you lose information. If it's too big, you get more latency. 
		- U store those representations in a *vector database* -> storing vectors efficiently, allowing fast retrieval
	- The user prompt
		- You also embed the user prompt with the same algorithm
	- Techniques
		- chunking
			- In the vector db you store the embedding of the full document + a chapter level vector
		- `HyDE`
			- Hypothetical document embeddings -> use the user query to generate a fake hallucinated document, embed that, and compare it to the vector in the vector database
# Agentic AI Workflows
- How could we extend the capabilities of LLMs from performing single tasks (enhanced with external knowledge) to handling multi-step, autonomous workflows?
- Definition
	- A process where an LLM-based application executes *multiple steps* to complete a task

// 55:31
https://www.youtube.com/watch?v=k1njvbBmfsw

# Case Study
# Multi-Agent Workflows
# What's next in AI