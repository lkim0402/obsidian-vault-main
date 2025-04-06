
>[!Step functions]
>Allows you to create reliable pre-defined execution sequences of different pieces of code
>- You define multiple steps for tasks that should be executed

- diagram
- Stars by defining some steps
	- Which input/output, which code should be executed
		- very commonly a [[Lambda]] function
- Combine the different steps
	- control how they are executed in relation to each other
	- **pre-defined** order
	- multiple steps in parallel/alternatives/conditionally, etc
- Example image
	![[Pasted image 20250322213445.png]]
	- every box is a step that executes some code, then passes the output to the next step which receives it as input