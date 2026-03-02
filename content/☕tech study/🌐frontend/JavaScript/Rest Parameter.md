
>[!Overview]
>Allows a function to accept an indefinite number of arguments as an array
> - You can then access all the passed arguments as an **array**
> - Used when you don't know how many parameters you want to put in

# Syntax
```js
function f(a, b, ...theArgs) {
  // …
}
```
- A function definition can only have one rest parameter
- The rest parameter must be the last parameter in the function definition
- Cannot have a [[Default Parameters|default value]]
# Example
```js
function sum(...theArgs) {
  let total = 0;
  for (const arg of theArgs) {
    total += arg;
  }
  return total;
}

console.log(sum(1, 2, 3));
// Expected output: 6

console.log(sum(1, 2, 3, 4));
// Expected output: 10
```
- Used to capture all arguments 
- It's an array!
	- You can use all [[Array Methods|array methods]]
- primary use case is for creating generic factory functions that accept any function as input and return a new version of the function with some specific modification
	- _**higher-order function**_: a function that accepts a function and/or returns a function
		- very common in js

Example: logged function factory
```js
function log(inputFunction) {
	return function(...args){
		console.log("Input", args);
		const result = inputFunction(...args);
		console.log("Output", result);
		return result;
	}
}

const f = log((a,b) => a + b);
f(1,2);
```
