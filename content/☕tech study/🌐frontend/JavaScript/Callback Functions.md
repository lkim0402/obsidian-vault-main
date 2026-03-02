
>[!Overview]
>Functions passed as arguments to other functions, which is then invoked inside the outer function to complete some kind of routine or action
>- Important for asynchronous programming

- The *consumer* of a callback-based API writes a function passed into the API
- The *provider / caller* of the API takes the function and "calls back" or executes the function at some point in the caller's body
	- The caller is responsible for passing the right parameters into the callback function
	- The caller may also expect a particular return value from the callback function, which is used to instruct further behavior of the caller
- 2 ways the callback may be called
	- *synchronous*
		- called immediately after the invocation of the outer function w/ no intervening asynchronous tasks
	- *asynchronous*
		- asynchronous callbacks are called at some point later, after an asynchronous operation has completed
		- [[setTimeout and setInterval]]
# Example 1
```JS
/// ======= 1st scenario =======
var callback = function()
{
	console.log("Done!");
}

// waits 5 seconds and prints "Done!" when the 5 seconds are up
setTimeout(callback, 5000);

/// ======= 2nd scenario =======
setTimeout(function()
{
	console.log("Done!");
}, 5000)
```
- just functions that get executed when something happens (like a click) or when some operation finishes (like a timer)
# Example 2
```js
// ===================================== another example

function useCallback(fn) {
    fn("Hello");        // This runs first
    fn("How are you?"); // This runs second
    fn("Goodbye!");     // This runs third
}

function printMsg(message) { // the callback
    console.log(message);
}

useCallback(printMsg);
```