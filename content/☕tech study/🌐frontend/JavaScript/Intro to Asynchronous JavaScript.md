>[!Asynchronous programming]
>- A technique that enables your program to start a potentially long-running task and still be able to be responsible to other events while that task runs, rather than having to wait until that task has finished
>- Once the task has finished, your program is presented with the result

- Event handlers are a form of asynchronous programming = you provide a function (the event handler) that will be called, not right away, but whenever the event happens
	- [[DOM (Document Object Model)]]
	- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Events
- Event handler is a particular type of callback (function passed into another function)
	- [[Callback Functions]]
	- But modern asynchronous APIs don't use callbacks, instead the foundation is **Promise**
	- [[Promises]]