```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>People entered:</h1>
    <h2 id="count-el">0</h2>
    <button id="increment-btn" onclick="increment()">INCREMENT</button>
    <script src="index.js"></script>
  </body>
</html>
```

`index.js`
```js
let count = 0;
function increment() {
  //   count++;
  //   document.getElementById("count-el").innerText = count;
  document.getElementById("count-el").innerText = ++count;
}
```
- Object types VS primitive types
	- Object types are passed by reference
		- using `let countEl = document.getElementById("count-el");` will work too!
		- `countEl` is an object so it holds a reference, meaning it's like a pointer or an address that points to the actual element object in the browser's memory (the DOM)
	- Primitive types are passed by value
		- `countEl.innerText = ++count;`
		- - `++count` runs first. `count` becomes `1`, and the value `1` is returned, and the browser takes a copy of that value
- `document.getElementById`
	- Working with the [[DOM (Document Object Model)]]

# Using Event Listener
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link href="style.css" rel="stylesheet" />
  </head>
  <body>
    <input type="text" id="input-el" />
    <button id="input-btn">SAVE INPUT</button>
    <script src="index.js"></script>
  </body>
</html>
```

```js
let inputBtn = document.getElementById("input-btn");
inputBtn.addEventListener("click", function () {
  console.log("Button clicked!");
});
```