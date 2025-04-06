### Functions
Two MAIN ways: named functions and anonymous functions

Named functions
```JS
// Named functions!
function greet(name)
{
	return "Hello " + name + "!"; 
}
console.log(greet("Leejun")); // "Hello Leejun!"
```

Anonymous functions
```js
// anonymous functions!
var greet = function(name)
{
	return "Hello " + name + "!"; 
}
console.log(greet("Leejun")); // "Hello Leejun!"
```

Immediately Invoked function expression (IIFE)
```js
const result = (function(a,b) {
	return a + b
}) (3,4);
console.log(result) // 7
```

### Functions within functions
```js
function createFunction() {
	
	function f(a,b) {
		return a + b // returns the sum
	}
	
	return f; // returns the function above
}
const f = createFunction(); //returns the function f
console.log(f(4,5)) // 9
```

### Returning objects that has functions
- [2704 - To Be Or Not To Be](https://leetcode.com/problems/to-be-or-not-to-be/description/?envType=study-plan-v2&envId=30-days-of-javascript)
- `toBe` is a key, and its value is a function.
	- This is called a method inside an object.
```js
/**
 * @param {string} val
 * @return {Object}
 */
var expect = function(val) {

    const obj = {
        toBe: function(val2) {
            if (val === val2) return true
            throw new Error("Not Equal");
        },

        notToBe: function(val2) {
            if (val !== val2) return true
            throw new Error("Equal")
        }
    }

    return obj;
    
};

/**
 * expect(5).toBe(5); // true
 * expect(5).notToBe(5); // throws "Equal"
 */
```
### Function hoisting
```js
function createFunction() {
    return f;
    function f(a, b) {
        const sum = a + b;
        return sum;
    }
}
const f = createFunction();
console.log(f(3, 4)); // 7
```
- JavaScript has a feature called hoisting where a function can sometimes be used before it is initialized. You can only do this if you declare functions with the function syntax.
- In this example, the function is returned before it is initialized. Although it is valid syntax, it is sometimes considered bad practice as it can reduce readability.

### Closure
>- JS closures are functions that can access values outside of their own curly braces
>- When a function is created, it has access to a reference to all the variables declared around it, also known as **lexical environment**. The combo of the function and its environment is called a **closure**.

- fireship: https://youtu.be/vKJpN5FAeF4?si=Bawt6sjOUV-ZEvb1
```js
function createAdder(a) {
	function adder(b) {
		return a + b;
	}
	return adder;
}
const f = createAdder(3);
console.log(f(5)); // 3 + 5 = 8
```
- `createAdder`passes the 1st parameter `a` and the inner function has access to it
- `createAdder` serves as a factory of new functions, which each returned function having different behavior

> A closure is created when a function is defined inside another function, and the **inner function references variables in the outer function's scope**. When the inner function is returned from the outer function, it retains a reference to the outer function's scope, and can continue to access those variables even after the outer function has finished executing. Vice-Versa is not true!!
```js
function outer() {
    let x = 10; // x is created
    return function inner() {
        console.log(x); // inner "remembers" x
    };
}

const myFunc = outer(); // outer() runs, x = 10, and returns inner
myFunc(); // prints 10 because inner() still remembers x
```
- Here, `inner` forms a **closure** over `x`, keeping access to it even though `outer()` has already executed.

#### Example 1
```js
/**
 * @param {number} n
 * @return {Function} counter
 */
var createCounter = function(n) {
    return function() {
        return n++;
    };
};

/** 
 * const counter = createCounter(10)
 * counter() // 10
 * counter() // 11
 * counter() // 12
 */
```
- it returns a value and **then** increments it
- so this is true encapsulation because in theory you can't have access to n
#### Example 2- trick question!

using `var`
```js
for (var i = 0; i < 3; i++) {
   const log = () => {   // <-- Closure starts here
       console.log(i);    // <-- `log` captures `i`
   };                    // <-- Closure ends here
   setTimeout(log, 100); // 3,3,3
}

```
- `log` **captures** the variable `i`, which is declared using `var` (function-scoped).
- `var` is hoisted up, meaning it's declared **once** for the entire function.
- All iterations of the loop reuse the same `i`
- Each time we define `log` it captures the same `i` (which keeps changing)
- By the time `setTimeout` executes, the loop has already finished, and `i` is 3, so every log function prints 3
✅ **Closures capture references to variables, not their values at the time of capture.**
	- So this is why it always gets the latest value!

using `let`
```js
for (let i = 0; i < 3; i++) {
   const log = () => {   // <-- Closure starts here
       console.log(i);    // <-- `log` captures `i`
   };                    // <-- Closure ends here
   setTimeout(log, 100);
}
```
- `let i` is **block-scoped**, meaning a **new `i` is created for each loop iteration**.
- Each iteration creates a **new, independent `i`**, which only exists in that specific iteration.
- The function `log` forms a **closure** over that specific `i`, so it remembers the value from that iteration.
- Even though `setTimeout` runs later, `log` still remembers the correct `i` for each iteration.
### Callbacks
- functions passed as arguments to other functions
- important for asynchronous programming
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

// ===================================== another example

function useCallback(fn) {
    fn("Hello");        // This runs first
    fn("How are you?"); // This runs second
    fn("Goodbye!");     // This runs third
}

function printMsg(message) {
    console.log(message);
}

useCallback(printMsg);
```
- just functions that get executed when something happens (like a click) or when some operation finishes (like a timer)
### Arrow functions
- behavior is just a regular function (anonymous functions w/ special syntax)
- Useful for one-liner functions
- often used as callbacks of native JS functions like map, filter or sort
- `() => {}`
```JS
// normal function 
function greet(name) {
	return "hi " + name + "!";
}
```
- normal functions are **hoisted**, meaning I can call this before the function is declared

Function expressions
- functions are assigned to a variable
- NOT hoisted
```js
const greet = function(name){ //only the variable is hoisted!
	return "hi " + name + "!";
}

// arrow function
const greet = (name) => {
	return "hi " + name + "!";
}
// CAN REMOVE () if only 1 parameter
const greet = name => { 
	return "hi " + name + "!";
}
// Can do this if we only have ONE LINE and want to do a explicit return
// note: no need to even write 'return'
const greet = name => "hi " + name + "!";

console.log(greet("Leejun")) // "hi Leejun!"
```

#### Simple example
Using an arrow as a callback compared to a normal function
```JS

let nums = [1,2,3,4,5,6]

// old way
function multTwoNum(num)
{
	return num * 2;
}

let multipliedNums = numbers.map(multTwoNum);
console.log(multipliedNums); // [2,4,6,8,10,12]

// using ES6 arrow functions
const multTwoNum = num => num * 2;
const multipliedNums = numbers.map(multTwoNum)

```

## Rest Arguments & Higher-order functions
> You can use **rest** syntax to access all the passed arguments as an **array**
```js
function f(...args) {
    const sum = args[0] + args[1];
    return sum;
}
console.log(f(3, 4)); // 7
// arg = [3,4]
```
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
