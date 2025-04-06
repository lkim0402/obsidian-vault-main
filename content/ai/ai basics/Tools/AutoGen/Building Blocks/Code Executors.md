> A component that takes input messages (e.g., those containing code blocks), performs execution, and outputs messages with the results.

- Inside an agent
- Executes the code -> returns results
- Two built in code executors
	- Command Line Code Executor: runs code in cmd env (Unix shell)
	- Jupyter Executor: runs in a Jupyter env
- Execute code 2 ways
	![[Pasted image 20240813135256.png]]
	- Locally: Run directly on the host system
	- Docker container: Runs code in an isolated conainer
		- ![[Pasted image 20240813135613.png]]
		- More secure when using production work
- Process
	![[Pasted image 20240813135154.png]]
	- The code executor will write to a code file, then execute that code file. Then it will read the console output of the code execution, and it's sent as a reply message.
	- Everything triggered by the message the agent receives