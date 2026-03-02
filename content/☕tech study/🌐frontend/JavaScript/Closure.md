
>[!Overview]
> JS closures are functions that can access values outside of their own curly braces
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

## Example 1
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
## Example 2- trick question!

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