- **Default parameters** let you set a fallback value for function parameters in case the caller doesn't provide one
- Only `undefined` triggers the default
# Basic syntax
```js
function greet(name = "Guest") {
  console.log(`Hello, ${name}!`);
}

greet("Alice");  // Hello, Alice!
greet();         // Hello, Guest!  (uses default)
```

Old way (manually)
```js
function greet(name) {
  if (name === undefined) {
    name = "Guest";
  }
  console.log(`Hello, ${name}!`);
}
```

Can be expressions
```js
function getID(id = Math.random()) {
  return id;
}

getID();     // Random number like 0.547382
getID(123);  // 123
```