
> [!Middleware]
> Like a set of **functions that run between receiving a request and sending a response**. 
>	- between raw requests and the response 
>	- common in lots of [[Node.js]] + [[Express.js]] based applications
>	- It’s a way to handle common tasks, like checking authentication, logging, or modifying the request/response.

# What can it do
- pre-process request
	- maybe it can change some aspects of the request/perform functions on the request before it goes to its final routing
	- ex) bodyparser
- logging the requests
	- how long did it take the request to come thru? what type of request? status of request being handled? 
	- ex) morgan
- authentication
	- is request valid? did it come from an authorized client
- errors
	- identify and handle errors before they go to the route handlers
- there can be multiple middleware (very common)
	- as many as u want
	- middleware runs in the order u define it
	- can be global (for all routes) or route specific
# Body parser
>[!Body parser]
>- commonly used middleware in node + express for **pre-processing**
>- looks at request bodies in a middleware before ur handlers
>- gives ALL requests a new `req.body` property!!
>- very commonly used to handle form data
#### Installation
- https://www.npmjs.com/package/body-parser
#### Example
<form action="/submit" method="POST">
  <label for="street">Street Name:</label>
  <input type="text" name="street" required>
  <label for="pet">Pet Name:</label>
  <input type="text" name="pet" required>
  <input type="submit" value="Submit">
</form>

```html
<form action="/submit" method="POST">
  <label for="street">Street Name:</label>
  <input type="text" name="street" required>
  <label for="pet">Pet Name:</label>
  <input type="text" name="pet" required>
  <input type="submit" value="Submit">
</form>
```
- in the form tag ([[== HTML ==#Form|html forms]])
	- **`action="/submit"`**: This tells the form to send a **POST request** to the `/submit` route when the form is submitted.
	- **`method="POST"`**: Specifies that the form's data will be sent as a **POST** request.
	- The form contains two input fields, one for "street" and one for "pet", which will be sent to the server when the form is submitted.
		- `name="street"` and `name="pet"`
```js
import express from "express";
import { dirname } from "path";
import { fileURLToPath } from "url";
import bodyParser from "body-parser"; // importing bodyParser

const __dirname = dirname(fileURLToPath(import.meta.url));

const app = express();
const port = 3000;

// == mounting the middle ware ==
// urlencodd({ extended: true }) creates a body for
// URL-encoeded requests like our form submission
app.use(bodyParser.urlencoded({ extended: true }));

app.post("/submit", (req, res) => {
  console.log(req.body);
});

app.get("/", (req, res) => {
  res.sendFile(__dirname + "/public/index.html");
});

app.listen(port, () => {
  console.log(`Listening on port ${port}`);
});
```
- mounting the middleware
	- path first, then middleware
	- Means the routes in the routers will be matched ONLY when the path starts with "`/<path>`"
		-  `app.use(indexRouter);` -> all paths
		- `app.use('/', indexRouter);`
		- `app.use('/users', usersRouter);`
- `app.use(bodyParser.urlencoded({ extended: true }));`
	- converts the raw data in the request body into a JavaScript object that can be easily accessed via `req.body`
	- we tell what type of data we want it to pass. since we're dealing with html form
# Morgan
>[!Morgan]
>- commonly used in middleware for logging
>	- logs the requests that come in your [[Backend Explained#Server|server]]
>	- import it, `app.use(morgan("combined"))`
- number of details on how detailed u want the logging to be
	- Under Predefined formats in the docs

```js
import express from "express";
import morgan from "morgan";
import { dirname } from "path";
import { fileURLToPath } from "url";

const app = express();
const port = 3000;

const __dirname = dirname(fileURLToPath(import.meta.url));

app.get("/", (req, res) => {
  console.log("page shown!");
  res.sendFile(__dirname + "/public/index.html");
});

//mounting morgan
app.use(morgan("combined"));

app.post("/submit", (req, res) => {
  res.send("Form succesfully submitted!");
  console.log("Submitted!");
});

app.listen(port, () => {
  console.log(`Listening on port ${port}`);
});

```

```html
 <form action="/submit" method="POST">
      <label for="street">Street Name:</label>
      <input type="text" name="street" required />
      <label for="pet">Pet Name:</label>
      <input type="text" name="pet" required />
      <input type="submit" value="Submit" />
</form>
```
- `action="/submit"` redirects to `localhost:3000/submit`, unless in the corresponding method (in this `POST`) nothing is sent back -> then page will load indefinitely

# Using multiple middlewares
- `logger` is a [[Custom middleware]]
```js
import express from "express";
import bodyParser from "body-parser";
import { dirname } from "path";
import { fileURLToPath } from "url";

const app = express();
const port = 3000;
const __dirname = dirname(fileURLToPath(import.meta.url));

function logger(req, res, next) {
  console.log("Request url: ", req.url);
  console.log("Request method: ", req.method);
  next();
}

// custom middleware
app.use(logger);

app.get("/", (req, res) => {
  res.sendFile(__dirname + "/public/index.html");
});

// middleware bodyparser
app.use(bodyParser.urlencoded({ extended: true }));

app.post("/submit", (req, res) => {
  res.send(`
    <p>Street name: ${req.body.street}</p>
    <p>Pet name: ${req.body.pet}<p>`);
});

app.listen(port, () => {
  console.log(`Listening on port ${port}`);
});

```

- `next()`
	- Without it, your request will be left hanging
	- It allows the request to continue to the next middleware/route handler
- A new `app.use` does NOT disable previous middleware