- matching routes = http method + url path
	- `GET /, POST /submit`, etc
- Essentially, middleware functions (like the one you wrote) run before the route handlers, and they can either:
	- End the request (with `res.send`, `res.json`, etc.).
	- Pass control to the next matching route or middleware with `next()`.
- `next()` 
	- passes control to the next matching route or middleware
	- if u dont use it, request never moves past the middleware & it will hang forever
- this is why the order of middleware is important
```js
import express from "express";
import { dirname } from "path";
import { fileURLToPath } from "url";

const app = express();
const port = 3000;
const __dirname = dirname(fileURLToPath(import.meta.url));

app.use((req, res, next) => {
  console.log("Request url: ", req.url);
  console.log("Request method: ", req.method);
  next(); // proceeds to next step of handling that rewuest
});

app.get("/", (req, res) => {
  console.log("page shown!");
  res.sendFile(__dirname + "/public/index.html");
});

app.post("/submit", (req, res) => {
  res.send("Form succesfully submitted!");
  console.log("Submitted!");
});

app.listen(port, () => {
  console.log(`Listening on port ${port}`);
});

```

or a defined function
```js
// app.use(logger);
function logger(req, res, next) {
  console.log("Request url: ", req.url);
  console.log("Request method: ", req.method);
  next();
}

app.use(logger);
```

