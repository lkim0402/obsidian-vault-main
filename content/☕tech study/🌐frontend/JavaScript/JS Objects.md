- Gives the ability to store data in depth
	- Complex/ composite data types
```js
let player = {
// key: value
  name: "Leejun",
  chips: 145,
  sayHello: function() {
	  console.log("Hello!");
  }
};
```
- how to get the values
	- `player.name` - dot notation
	- `player["name"]` - bracket notation
- You can put anonymous functions like `sayHello`
	- [[Different ways of writing functions]]