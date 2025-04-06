
>[!HTTP]
>>HyperText Transfer Protocol
>- A language that allows computers to talk to each other across the internet
>- primarily go **from client to server**, but the server responds back to the client — so there’s communication in both directions
>	- related: [[Backend Explained#Client-side vs Server-side|client-side and server-side]] 
>- we wanna know enough vocab to get started with the computer language HTTP

Clarifications
- HTTP Methods are Universal, used across different programming languages + frameworks
- The core HTTP methods (GET, POST, PUT, PATCH, DELETE) remain the same across platforms
	- [[Python]] (flask) would implement it differently than [[Express.js]]

# HTTP Request components
1. **HTTP Method**: Specifies the action to be performed (GET, POST, PUT, DELETE, etc.).
2. **URL**: Specifies the resource on the server (e.g., `https://api.example.com/users`).
3. **Headers**: Metadata about the request (such as content type, authorization).
4. **Body**: Data sent with the request (e.g., user information in a POST request).
#### Examples
Example of an HTTP Request (GET)
```http
GET /users HTTP/1.1
Host: api.example.com
Authorization: Bearer <token>
Accept: application/json
```

Example of an HTTP Request (POST)
```http
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer <token>
{
  "name": "John Doe",
  "email": "john@example.com"
}

```
# HTTP Request Methods
> In the perspective of the clients (often the user in a web browser) making a request to the server.
> - Part of how you define [[APIs]]
- `GET`
	- request a resource from a [[Backend Explained#Server|server]], you are getting something from the server!
	- [[== HTML ==|HTML]] website, text, piece of data
	- You, the client using the browser, are trying to GET a resource (ex. a homepage)
- `POST` 
	- sending a resource to the server (a form, email & pw, clicking sign up button, etc)
- `PUT` 
	- used to update a resource completely, and you are sending the entire updated resource
	- send ALL of the data for the user
- `PATCH`
	- partially update a resource, you only send the fields you want to update, and the rest of the resource remains unchanged
- `DELETE`
	- deletes resources from the server or [[Backend Explained#Database|database]]
	- this is a request from the [[Backend Explained#Client-side vs Server-side|client-side computer to the server-side computer]] saying that there is something needed to be deleted
# In Express.js
- provides a convenient way to define routes and handle these HTTP methods in a [[Node.js]] server
	- Express handles incoming HTTP requests and abstracts that for you, you aren't manually writing raw HTTP requests
- [[Express.js]]
#### GET
```js
app.get("/", (req, res) => {
  console.log(req.rawHeaders);
  // res.send("Hello World");
  res.send("<h1>Hello</h1>");
});
```
- request
	- comes in from the browser, request is made to a **specific URL or route** on the server
- response
	- whats more important is sending a response
- `rawHeaders`
	- list of key/value pairs that come from where the request originated
	- ex. Google Chrome, Windows, localhost:3000, etc etc

#### figuring out path dynamically
- using res.sendFile
- requires exact path

```js
import express from "express";
import { dirname } from "path";
import { fileURLToPath } from "url";
const __dirname = dirname(fileURLToPath(import.meta.url));

const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.sendFile(__dirname + "/public/index.html");
});

app.listen(port, () => {
  console.log(`Listening on port ${port}`);
});
```


## /Endpoints
>[!Endpoints]
>- Refers to the specific URLs or paths that the server provides to handle different types of requests. 
>- Each endpoint is a "route" in your application where a specific action is performed based on the request method (GET, POST, PUT, DELETE, etc.).
- We can use different endpoints to plan for when a client tries to hit that endpoint
	- we can easily build multiple page websites, navigation into our web apps
- Examples
	- `/` => `localhost:3000/` or  `localhost:3000`
	- `/about` => `localhost:3000/about`

```js
app.get("/about", (req, res) => {
  res.send("<h1>This is the about page!</h1>");
});
```

## HTTP Response
- The status codes are from the **client’s perspective**, telling the client what happened with its request.
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status
	![[{21D2CE85-BB34-4B18-A749-76B11FA27B23} 1.png]]
- The cheatsheet
	![[{AFC430C7-5A87-4BBE-A42D-87F5FD740528}.png]]
- Succesful
	- `200 OK`
		- the most common we'll be sending and using
		- everything went OK
- we can use `res.sendStatus(code)` to send back a status code
```js
app.post("/", (req,res) => {
	res.sendStatus(201);
})
```

# Postman
- can test requests and endpoints without the [[== Frontend ==|frontend]]