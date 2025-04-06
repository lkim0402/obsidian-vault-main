
>[!JSON]
>[[== JavaScript ==|JavaScript]] Object Notation
>-  **text-based format** for storing and exchanging data over the internet in a readable & efficient way

## JavaScript Object vs JSON
- diagram
	![[{0FB7AACB-2779-4596-A348-54C0490DDF57}.png]]
- **JavaScript Object**
	- A **JS object** is a data structure in JavaScript used to store collections of data in the form of key-value pairs. 
	- `const data = { name: "Alice", age: 30 };`
- **JSON (JavaScript Object Notation)**
	- **JSON** is a **string** representation of data, based on JavaScript object syntax. It’s used for data exchange between servers and clients. It's **text-based**.
	- `{ "name": "Alice", "age": 30 }`
	- Exists **only when it's serialized** -> u convert a JavaScript object to a JSON thru serialization!
### The process
```js
const data = { name: "Alice", age: 30 };

// 1. Serialization (turning object to string)
const jsonData = JSON.stringify(data); //'{"name":"Alice","age":30}'

// 2. Send this jsonData over the network

// 3. Receiving this jsonData & Desserialization
const receivedData = JSON.parse(jsonString);
console.log(receivedData); // { name: "Alice", age: 30 }
// receivedData is back to being a JavaScript object, ready to be used in your program.
```
- When you send data over the internet, you send the serialized JSON string.
- Serialization: `JSON.stringify()`
- Deserialization: `JSON.parse()`
