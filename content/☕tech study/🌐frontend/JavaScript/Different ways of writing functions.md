- Functions are considered "first class citizens"
	- They have same capabilities as other data types => functions can be assigned to variables, or be passed as arguments to other functions
# Main Ways: named/anonymous
## Named functions
```JS
// Named functions!
function greet(name)
{
	return "Hello " + name + "!"; 
}
console.log(greet("Leejun")); // "Hello Leejun!"
```
## Anonymous functions
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

# Functions within functions
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

# Returning objects that has functions
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
# Function expressions
- functions are assigned to a variable
	- NOT hoisted
	- [[Function Hoisting|normal functions are hoisted]] (I can call this before the function is declared)
```js
// a function expression
const greet = function(name) { //only the variable is hoisted!
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
- ***arrow functions***
	- behavior is just a regular function (anonymous functions w/ special syntax)
	- Useful for one-liner functions
	- often used as callbacks of native JS functions like map, filter or sort
	- `() => {}`

## Simple example
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

