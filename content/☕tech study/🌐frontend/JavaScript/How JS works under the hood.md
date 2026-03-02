-  JavaScript Runtime Environment (eg. V8 engine)
	- The Heap
		- It handles memory allocation for us so we don't have to
	- The Call Stack
		- It's where your code *actually runs*
	- As JavaScript is single threaded, the call stack can only execute one piece of code at a time in its single thread
# JS is single-threaded and non-blocking
- Single-threaded
	- JS can only do one thing at a time - only 1 "thread" of execution 
- Non-blocking
	- Even though JS is single-threaded, it doesn't get "stuck" waiting for slow operations (like fetching data, reading files, or timers)
	- Instead of waiting, JS says it will come back later and moves on to the next task before returning
```js
console.log("Start");

setTimeout(() => {
  console.log("This runs later");
}, 2000);

console.log("End");

// Output:
// Start
// End
// This runs later (after 2 seconds)
```
- There are helping hands that makes JS non-blocking despite it being single threaded
	- Call Stack (JavaScript Engine)
		- It's where your code *actually runs*
		- As JavaScript is single threaded, the call stack can only execute one piece of code at a time in its single thread
		- Think: the cashier actively helping a customer
	- Web APIs (Browser) / C++ APIs ([[Node.js]])
		- Handle async operations **outside** JavaScript
		- Examples: `setTimeout`, `fetch`, DOM events/manipulation, file reading, etc
		- Think: the kitchen cooking food while the cashier helps others
	- Task Queue (Callback Queue)
		- Where completed async operations wait their turn
		- Think: the pickup counter where finished orders wait to be called
	- Event Loop
		- Constantly checks: "Is the call stack empty?"
		- If yes, it takes the first task from the queue and puts it on the stack
		- Think: the person calling out "Order #5 ready!"

