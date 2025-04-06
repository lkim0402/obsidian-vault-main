
>[!Node.js]
>It's an asynchronous event-driven [[== JavaScript ==|JS]] runtime, designed to build scalable network applications
>	- Actually not a framework...
>	- Node.js is a **runtime environment** that allows you to run JavaScript code **outside of a browser**.
- diagram
	![[Pasted image 20250308212419.png]]
- A runtime environment
	- when js was first created, it was designed to run in the browser (impossible to use js for anything not a website).
	- but Node uses the V8 engine (comes from chromium, written in C and [[C++]], VERY fast, powers the chrome browser)
	- Node allows us to use JS outside browsers, maintaining our full JS stack from [[== Frontend ==|frontend]] to [[== Backend ==|backend]]
- asynchronous, event-driven
	- js code doesn't have to do everything sequentially
- In summary, we need node because it allows us to build an [[Backend Explained#Application|application]] on a computer using JS
	- this application will be running on our [[Backend Explained#Server|server (a computer, which is NOT a browser)]], and Node.js allows that to happen
- **You need a deployment platform** to host your app so users can access it.
	- AWS, Vercel, etc
- Some redditor
	![[{F769F135-C8BD-4AC1-95D6-D681E3144E88}.png]]
	- https://www.reddit.com/r/learnjavascript/comments/3d4hs5/comment/ct2b9oe/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button
# Commands
- `node -v`
- Node REPL (Read Eval Print Loop)
	- A computer environment where user inputs are read and evaluated, and then the results are returned to the user
	- similar to JS console in browser.. we've just taken that out of the browser and run it in our pc terminal
	- `node`
		- `.help`
		- `.exit` or `Ctrl+C`
	- (not unique to node, smth we can do with most programming languages)
- writing a js file and using node to run the entire file
	- `node index.js`

# Native Node Modules
- Your starting tool set. Has TONS of FEATURES for creating applications (esp backend) to be easier
	- file access (reading and writing), networking, etc
	- https://nodejs.org/docs/latest-v18.x/api/index.html
- **Built-In Libraries/Modules**
    - **Core Modules**: Node.js comes with a **rich set of built-in libraries** (e.g., `http`, `fs`, `path`, `os`, `stream`, etc.) to handle basic tasks like HTTP requests, file system operations, networking, and more, without needing extra libraries.
    - This means you can create basic web servers, manage files, and do networking tasks without needing third-party libraries.
- **NPM (Node Package Manager)**
    - **NPM** is the world's largest package registry, which means there’s a massive selection of **open-source libraries** and packages available. You can install packages for anything, like authentication, database integration, utilities, and more.
    - You can use npm to manage dependencies and add functionality to your app easily.
- cross platform

### File System
> The native node module that allows us to access the local storage
- https://nodejs.org/docs/latest-v18.x/api/fs.html
- remember, with native JS run in the browser,  you can't access a user's files in their PC
- but now that we can use JS outside the browser, we can read/write files on the server (or local computer)
- ways
	- import the code
	- require the bits that we need from this module

require
```js
const fs = require("")
```
- module we need needs to be entered as a string

```js
const fs = require("fs");

// writes to file
fs.writeFile("message.txt", "Hello node!", (err) => {
  if (err) throw err;
  console.log("File saved!");
});

// reads from file
fs.readFile("message.txt", "utf8", (err, data) => {
  if (err) throw err;
  console.log(data);
});

```
- allows us to take a msg (ex. user input) and write it into a file to be saved onto the computer
- [writeFile](https://nodejs.org/docs/latest-v18.x/api/fs.html#fswritefilefile-data-options-callback), [readFile](https://nodejs.org/docs/latest-v18.x/api/fs.html#fsreadfilepath-options-callback)

# Node Package Manager (NPM)
- Native node modules - toolbox we have at home
- NPM (node package manager) - community tool library
	- its open source and you can search to find all the packages that other developers have created
	- comes pre-bundled with node, so if u have node installed you also have npm installed
- initializing first npm project
	- `npm init`
	- this will create the configurations for our project -> `package.json`
		- similar to how js objects are structured
- we can start installing and using npm packages
	- will be added to dependencies in `package.json` and a separate folder in `node_modules`
	- `npm install <package1> <package2>`
	- `npm i <package>`
	- https://www.npmjs.com/
- The ECMAScript modules
	- CJS - CommonJS
		- we have to use the "require" keyword
	- ESM - ECMAScript
		- we can use "import" keyword (and whole bunch of behind the scene things)
		- we have to go to `package.json` and add `"type": "module",`
		- in the main file, write `import generateName from "sillyname";`
			- write `import xxx from "sillyname";` first, then write the xxx keyword (for autocomplete)
		- you CAN'T mix imports!
- if u copied some project, the node_modules will not be downloaded for u
	- u have to `npm install`
Example task from angela
```js
/* 
1. Use the inquirer npm package to get user input.
2. Use the qr-image npm package to turn the user entered URL into a QR code image.
3. Create a txt file to save the user input using the native fs node module.
*/

import input from "@inquirer/input";
import qr from "qr-image";
import fs from "fs";

// getting url from user input
const url = await input({ message: "Enter URL" });

// save user input to txt file
fs.writeFile("url.txt", url, (err) => {
  if (err) throw err;
  console.log("file saved!");
});

// creating the qr image from user entered url
const qr_img = qr.image(url);
qr_img.pipe(fs.createWriteStream("i_love_qr.png"));

```