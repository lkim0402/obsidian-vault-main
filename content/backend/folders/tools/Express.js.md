
>[!Express.js]
>A JavaScript framework that allows us to create [[Backend Explained|backend]] for our websites.
>- a backend framework for building APIs and handling requests
>- runs on top of [[Node.js]], and makes everything much easier.
>- You can’t use Express without [[Node.js]] because **Express is a web framework built for Node.js** (IT USES NODE)

- [[Next.js]] VS node.js + express.js
	- Next.js is great for full-stack web apps (especially those with a frontend)
		- needs frontend (blog, ecommerce, dashboards)
		- SEO benefits & static site generation (SSG/SSR)
		- mainly content-driven
	- Node.js + Express is better for backend-heavy projects where you don’t need a React frontend
		- API-focused, backend-heavy, or real-time apps
- why express?
	- u can use node to do a LOT of things, as it allows [[== JavaScript ==|JS]] to run on any computer, indeed its mostly used for backend, but also used for other stuffs like IoT, desktop apps (in fact VS Code is run using Node.js!!)
	- when we have a tool that generalizes for a lot of things, it often is NOT specialized in one specific thing -> especially a website backend
	- its like using a manual screwdriver (node.js), then using an electric screwdriver (express.js)
	- so express allows us to create a backend with js + node, but it makes the whole thing SO MUCH QUICKER & EASIER![[Pasted image 20250310144927.png]]
# The flow
- When you start the server:
	- The file is executed once
	- The server is set up
	- Route handlers and [[Express middleware|middlewares]] are defined
	- `app.listen()` starts the server and keeps it running
- For each incoming request:
	- Only the relevant middleware and route handlers are executed
	- The server remains running in memory
	- Your defined middleware and routes are called as needed
# Creating our first express server
- what we'll use
	- [[Backend Explained#Server|server]] - our local computer
	- application - express + node
- 6 steps in creating an express server
	- Create a directory (folder with all our project files)
	- Create `index.js` file
	- Initialize [[Node.js#Node Package Manager (NPM)|npm]] 
		- `"main" : "index.js"`
		- Add `"type": "module"` in `package.json` to use ES6
	- Install the express package in the directory
		- `npm i express`
	- Use the installed package and write the server application in `index.js`
	- Start server (and test what happens when we try to send a request to our server)
		- run node command: `node your_app.js`
		- go to http://localhost:3000/
```js
import express from "express"; // thanks to ES6

const app = express();
const port = 3000;

app.listen(port, () => {
	console.log("Server running on port 3000.");
})
```
- `const app = express();`
	- Creates an instance of an Express application
- `app.listen(port, callback)`
    - **`port (3000)`** → The port number where your server will be listening for requests (like a door on your computer).
    - **`callback function`** → Runs after the server starts successfully. Useful for logging or initialization.

# Localhost & Post
#### LocalHost
- A special address that points to your **own computer**.
	- basically making our own computer the server of our website's [[Backend Explained|backend]]
- **`http://localhost`** is the same as **`http://127.0.0.1`** — a loopback address that stays within your machine.
#### Port
- diagram
	![[{04D023AF-9980-424E-9F2A-79C43BB965F0}.png]]
- Think of your computer/server as an apartment building.
	- The **IP address** (like `127.0.0.1`) is the building's street address.
	- The application is sitting behind the door, waiting for a knock!
- There are thousands of ports (like doors) on your computer, and different applications use different ones:
	- **Port 3000** → Maybe your Express.js server.
	- **Port 5432** → A PostgreSQL database.
	- **Port 80** → Default HTTP web traffic.
- The flow
	- Server starts:
		- Your application (Express.js) binds to port 3000 — like the person standing behind the door, ready to answer knocks.
	- You visit localhost:3000:
		- Your browser sends an HTTP request — this is the knock on door 3000.
	- Express hears the knock:
		- Since your app is listening on port 3000, it hears the knock, checks what the request is (like "What do you want?"), and sends a response.
	- Response sent back:
		- Your app opens the door and hands you the response — maybe some HTML or a "Hello, world!" message.
- checking which ports on our computer are currently listening for interactions from the outside
	- Windows: `netstat -ano | findstr "LISTENING"`
	- Mac/Linux: `sudo lsof -i -P -n | grep LISTEN`

