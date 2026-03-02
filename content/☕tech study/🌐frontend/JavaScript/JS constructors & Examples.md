# Two types: inbuilt, custom
- Inbuilt
	- Part of JS
	- Provide objects in various predetermined formats, like `Date`, `Error` objects, and Objects for each data type
- Custom
	- Constructors that we design ourselves for our own purposes

# Useful inbuilt constructors
- These exist  but are rarely used
	- `String()`
		- [[Strings]]
	- `Number()`
	- `Array()`
	- `Object()` (except this, this is used)
	- `Boolean()`
## `Date()` constructor
- We instantiate a new `Date` object
```js
const dateObject = new Date();
console.log(dateObject); //2025-10-11T09:15:51.762Z
console.log(dateObject.toString()); // Sat Oct 11 2025 18:17:19 GMT+0900 (Korean Standard Time)
console.log(dateObject.getFullYear()); // 2025
```
- Alternatives like `luxon` exist because `Date` can be pain
## `Error()` constructor
- creates new `Error` objects
Syntax:
```js
new Error()
new Error(message)
new Error(message, options)
new Error(message, fileName)
new Error(message, fileName, lineNumber)

Error()
Error(message)
Error(message, options)
Error(message, fileName)
Error(message, fileName, lineNumber)
```

Examples
```js
const x = Error("I was created using a function call!");

// above has the same functionality as following
const y = new Error('I was constructed via the "new" keyword!');

// in try catch
try {
  frameworkThatCanThrow();
} catch (err) {
  throw new Error("New error message", { cause: err });
}
```
- the `throw` keyword ends the execution
