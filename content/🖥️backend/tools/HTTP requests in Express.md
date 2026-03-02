>[!What it does]
>[[Express.js]] provides a **convenient abstraction layer** over raw HTTP  a [[Node.js]] server:
>- Handles incoming HTTP requests automatically
>- Provides simple methods to define routes and responses
>-  You don't write raw HTTP - Express parses it for you

- [[HTTP Fundamentals]]
- [[Express.js]]
# Express Route Handlers
Express translates HTTP methods into JavaScript functions:
```java
// GET request handler
app.get("/", (req, res) => {
  console.log(req.rawHeaders);  // Express gives you parsed request data
  res.send("<h1>Hello</h1>");   // Express handles response formatting
});

// POST request handler  
app.post("/", (req, res) => {
  res.sendStatus(201);  // Express method to send status codes
});
```
- Express enhances the basic HTTP request/response with useful properties:
	- **`req.rawHeaders`** - Express-parsed header data
		- `rawHeaders` = list of key/value pairs that come from where the request originated
			- ex. Google Chrome, Windows, localhost:3000, etc etc
	- **`res.send()`** - Express method to send responses
	- **`res.sendFile()`** - Express method to send files
	- **`res.sendStatus()`** - Express method to send status codes
- request comes in from the browser, and is sent to a **specific URL or route** on the server

#### Endpoints
**Endpoints** = specific URL paths your Express server responds to
- Each endpoint is a "route" in your application where a specific action is performed based on the request method (`GET`, `POST`, `PUT`, `DELETE`, etc.).
- Express makes it easy to define multiple endpoints:
```js
app.get("/", (req, res) => {
  res.sendFile(__dirname + "/public/index.html");
});

app.get("/about", (req, res) => {
  res.send("<h1>This is the about page!</h1>");
});
```
- `/` → `localhost:3000/`
- `/about` → `localhost:3000/about`

```js
app.get("/about", (req, res) => {
  res.send("<h1>This is the about page!</h1>");
});
```
#### Dynamic File path
- using `res.sendFile` -> requires exact path

```js
import { dirname } from "path";
import { fileURLToPath } from "url";
const __dirname = dirname(fileURLToPath(import.meta.url));

app.get("/", (req, res) => {
  res.sendFile(__dirname + "/public/index.html");
});
```

---
# Summary
1. **HTTP** = The universal protocol/language ([[HTTP Fundamentals]])
2. **Express** = A [[Node.js]] framework that implements HTTP in [[JavaScript]]
### Key Distinction
- **HTTP methods** (GET, POST, etc.) are the **what** - universal concepts
- **Express handlers** (`app.get()`, `app.post()`, etc.) are the **how** - JavaScript implementation

## Quick Reference
- HTTP (Universal)
	- Methods: GET, POST, PUT, PATCH, DELETE
	- Status codes: 200, 404, 500, etc.
	- Request structure: Method + URL + Headers + Body
- Express (JavaScript Implementation)
	- Route handlers: `app.get()`, `app.post()`, etc.
	- Response methods: `res.send()`, `res.sendFile()`, `res.sendStatus()`
	- Request object: `req.headers`, `req.body`, etc.