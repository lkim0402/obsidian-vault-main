
>[!Langgraph]
>A new package within [[LangChain|LangChain's]] ecosystem that allows us to build customized agents and achieve complex tasks.
> - Building language agents with graphs; The entire lofic/flow of the agent can be expressed as a graph

- Flow engineering
	- Allows devs to define the scope which the LLM is going to be used within our agent
- We can achieve this in [[LangChain]] but doing it in LangGraph is much more abstract and flexible, and actually create a full cycle
	- We define the flow of our program and blend it with LangGraph
- supporting these agents out of many (all of them are cyclic graphs)
	- diagram
		- ![[langgraph oerview.png]]
	- [[ReAct (Reason + Act)]]
		- Synergizing Reasoning and Acting in Language Models
		- Early paradigm for building gents
	- Self-Refine
		- Iterative Refinement with Self-Feedback
	- `AlphaCodium`
		- From prompt engineering to flow engineering
		- creates coding agent using *flow engineering* -> a, systematic approach to designing multi-step, modular workflows for AI systems, shifting focus from single, static prompts to iterative, chain-of-thought processes
#  how to use
- [[ReAct (Reason + Act)|We first build a simple ReAct Agent from Scratch]] (without Langgraph)