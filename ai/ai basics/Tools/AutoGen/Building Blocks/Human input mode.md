> All Conversible Agents and its classes have `human_input_mode`
- `"ALWAYS"` 
	- the agent prompts for human input every time a message is received
	- Under this mode, the conversation stops when the human input is "exit", or when `is_termination_msg` is True and there is no human input
- `"TERMINATE"` (DEFAULT)
	- the agent only prompts for human input only when a termination message is received or the number of auto reply reaches the `max_consecutive_auto_reply`
	- [[terminate_mode.py]]
- `"NEVER"`
	- agent will **never** prompt for human input
	- human skip and trigger an auto-reply
	- the conversation stops when the number of auto reply reaches the `max_consecutive_auto_reply` or when `is_termination_msg` is True
	- [[never_mode.py]]

