# `setTimeout`
- sets a timer which executes a function or specified piece of code once the timer expires
## Syntax
```js
setTimeout(code)
setTimeout(code, delay)

setTimeout(functionRef)
setTimeout(functionRef, delay)
setTimeout(functionRef, delay, param1)
setTimeout(functionRef, delay, param1, param2)
setTimeout(functionRef, delay, param1, param2, /* …, */ paramN)
```

## Example
```js
// =========== EXAMPLE 1 ===========
function displayTrafficLight(light) {
	console.log(light);
}
const questionTimer = setTimeout(displayTrafficLight, 3000, "red");
document.getElementById("clear-btn").addEventListener('click', function() {
	clearTimeout(questionTimer);
})

// =========== EXAMPLE 2 ===========
// anonymous function
setTimeout(
  function (string) {
    console.log("test string: " + string);
  },
  3000,
  "this is a test string!"
);
```
- `clearTimeout()`
	- clears the timeout

You can use `performance.now()`, a really convenient tool for measuring performance
```js
const start = performance.now();

setTimeout(() => {
  const end = performance.now();
  console.log(`Execution time: ${end - start} milliseconds`);
});
```

```
[Running] node "c:\Users\Leejun\Desktop\full_stack_learning\full_stack_tutorial_2025\14.advanced_foundations\practice.js"
Execution time: 8.405500000000004 milliseconds
```

# `setInterval`
- Basically like `setTimeout` but it *runs repeatedly* at regular interals
```js
setInterval(callback, delay);
```

## Examples
- clock
```js
setInterval(() => {
  const now = new Date();
  console.log(now.toLocaleTimeString());
}, 1000);

// Updates every second:
// 3:45:01 PM
// 3:45:02 PM
// 3:45:03 PM
```
- auto save
```js
setInterval(() => {
  saveDocument();
}, 30000);  // Auto-save every 30 seconds
```

## Stop it using an ID
```js
const intervalId = setInterval(() => {
  console.log("Running...");
}, 1000);

// Stop it after 5 seconds
setTimeout(() => {
  clearInterval(intervalId);
  console.log("Stopped!");
}, 5000);
```