- [[Different ways of writing functions]]

>[!Overview]
>JavaScript moves declarations to the top of their scope **before** executing code. 
>- Different things are hoisted differently.
```js
greet();  // ✅ Works! Prints "Hello"

function greet() {
  console.log("Hello");
}
```
- Like the example, functions are fully hoisted
	- You can only do this if you declare functions with the function syntax.
- In this example, the function is returned before it is initialized. Although it is valid syntax, it is sometimes considered bad practice as it can reduce readability.

| Type                   | Hoisted? | Usable before declaration? |
| ---------------------- | -------- | -------------------------- |
| `function` declaration | ✅ Yes    | ✅ Yes                      |
| `var`                  | ✅ Yes    | ⚠️ Yes, but `undefined`    |
| `let` / `const`        | ✅ Yes    | ❌ No (TDZ)                 |
| `class`                | ✅ Yes    | ❌ No (TDZ)                 |
For `var`, it's hoisted but it's initialized to `undefined`:
```js
console.log(x);  // undefined (not an error!)
var x = 5;
console.log(x);  // 5

// It's like this:
var x = undefined;  // hoisted
console.log(x);
x = 5;
console.log(x);
```
## Temporal Dead Zone
- The TDZ is the period between when a variable is **hoisted** (recognized by JavaScript) and when it's **initialized** (given a value).
	- Variable and function *declarations* are moved to the top of their containing scope during the compilation phase, before code execution
- It's a safety measure to prevent you from accidentally using variables before you've properly declared them
```js
console.log(test);  // ❌ ReferenceError: Cannot access 'test' before initialization
let test = 0;
```
- What happens
	1. JavaScript knows `test` exists (it's hoisted)
	2. BUT it's in the TDZ - you can't use it yet
	3. Only after the `let test = 0;` line runs does it exit the TDZ
