- `Math.random()`
	- Range: 0 <= x < 1
```js
let randomNumber = Math.random() * 6
```
- Range is 0 <= x < 6

- With range
```js

function getRandomArbitrary() {
  let result = Math.random() * (max - min) + min;
  return Math.round(result);
}
```

- Example: 1-13
```js
function getRandomCard() {
  return Math.floor(Math.random() * 13) + 1;
}
```