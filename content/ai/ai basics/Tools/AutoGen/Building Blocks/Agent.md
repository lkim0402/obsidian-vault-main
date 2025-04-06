> An entity that can send messages, receive messages and generate a reply using models, tools, human inputs or a mixture of them
- Can be built on top of LLMs
- Has code executors that enables to execute code
- Human element: humans can be in the loop
- Example
	![[Pasted image 20240812164408 1.png]]
- Features
	- Conversable: Agents can send and receive messages
	- Customizable: Agents can integrate with AI models, tools, humans, or a combination of all
### Some Built-In Agents in AutoGen

^24fdf6

![[ai/_Tools/AutoGen 1/_Resources/Pasted image 20240812181446 1.png]]
- `ConversableAgent`
	- The main agent that can integrate AI models (tools and human input)
	- You can pass in certain settings (`human_input_mode`, `code_execution_config`, `DEFAULT_SYSTEM_MESSAGE`)
- `AssistantAgent`
	- Assistant agent, designed to solve a task with LLM (that you can attach).
	- A subclass of `ConversableAgent` configured with a default system message. The default system message is designed to solve a task with LLM, including suggesting python code blocks and debugging.
- `UserProxyAgent`
	- A proxy agent for the user, that can execute code and provide feedback to the other agents.
	- A subclass of `ConversableAgent` configured with `human_input_mode` to ALWAYS and `llm_config` to False.
	- Triggers when `human_input_mode="ALWAYS"`  
- `GroupChatManager`
	- A chat manager agent that can manage a group chat of multiple agents.

![[Pasted image 20240813144057.png]]





- `is_terminate_msg`
	- This condition can trigger termination if the _received_ message satisfies a particular condition, e.g., it contains the word “TERMINATE”. You can customize this condition using the `is_terminate_msg` argument in the constructor of the `ConversableAgent` class.
	- https://microsoft.github.io/autogen/docs/tutorial/chat-termination/
	- Example: `is_terminate_msg = lambda msg: "elephant" in msg["content"].lower()`