- You can add [[HTTP Fundamentals#HTTP Request Methods|HTTP methods]]
- `GET` is the default method
```js
const response = await fetch(
  "https://jsonplaceholder.typicode.com/todos/1",
  {
	method: "GET",
  }
);
```

- You can also add a body with `JSON.stringify`
	- Before we send our request, we need to convert it to [[JSON]]
```js
const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
  method: "POST",
  body: JSON.stringify({
	title: "foo",
	body: "bar",
	userId: 1,
  }),
});
```