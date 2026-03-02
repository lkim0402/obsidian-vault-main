
> [!Promise]
> An object that will **eventually** be returned by an asynchronous function, which represents the *current state of the operation*
> - All asynchronous functions return a promise.
> - [(amazing) MDN docs](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Async_JS/Promises)
 
- States include
	1. **Pending** - The initial state
		- Waiting to get the data back from the API: operation not yet succeeded or failed
	2. **Fulfilled** - Successfully resolved (`.then()` is called)
	3. **Rejected** - Operation failed (`.catch()` is called)
- Other states
	- **Completed** - Promise is no longer pending (either fulfilled or rejected)
	- **Resolved** - Promised is completed, or it's "locked in" to follow the state of another promise (advanced concept)
- A `Promise` constructor receives a single argument: an **executor function**
    - This executor function receives two callbacks: `resolve(value)` and `reject(reason)`
    - Inside the executor, you **must call either `resolve` or `reject`** to settle the promise
- With a promise-based API, the asynchronous function starts the operation and returns a `Promise` object
	- You can then attach handlers to this `Promise` object, and these handlers will be executed with the operation has succeeded or not
# Creating your own - `resolve(), reject()`
- functions you need to use when *creating your own promise*
- automatically provided to you by JavaScript when you create a new `Promise`
- `resolve()`
	- This function is called when the asynchronous operation inside the Promise succeeds, and it **fulfills** the Promise.
    - The result passed to `resolve` is what the **Promise** will resolve to (i.e., the value you get when the Promise is successful).
- `reject()`
	- This function is called when the asynchronous operation inside the Promise fails, and it **rejects** the Promise.
	- The argument passed to `reject` is usually an **error** or a message explaining why the operation failed.
- The names don't really matter (`resolve` or `reject`) but the ORDERING matters
	- You can have `new Promise((apple, banana))`
# `fetch()`
- `fetch()` function is a modern [[APIs|API]] used to make network requests, and it *always* returns a Promise
	- A function where you give a URL of an API, then it "fetches" the stuff from the URL
- The first Promise
	- Resolves to a `Response` object
	- Only contains metadata about the response (like status code `200` OK or `404` Not Found), but NOT the actual data you want
- The second Promise
	- To get the data (e.g. in [[JSON]] format), you *MUST* call another method on the `Response` object, such as `.json()`, which also returns a Promise
	- This second Promise resolves to the actual data (the parsed JSON)
# Handlers: `then()`, `catch()`, `finally()`
- They are handlers attached to a promise, and they accept [[Callback Functions]] that runs on success/failure
- `then()`
	- Receives a function to be executed when promise is fulfilled
	- allows you to do other things asynchronously while promise is not yet fulfilled!
	- each `.then()` **returns a promise**
- `catch()`
	- Receives a function to be executed when promise is **rejected** 
	- called when any of the asynchronous function calls fail
- `finally()`
	- Runs at the last regardless of success or failure, does not receive the success/failure value

Example of using `fetch` with`then` and `catch` handlers:
```JS
const fetchPromise = fetch('https://api.example.com/data')
  // This .then() handles the first Promise (the Response object)
  .then(response => {
    // Check if the HTTP request was successful
    if (!response.ok) {
      // If not, throw an error to trigger the .catch()
      throw new Error('Network response was not ok');
    }
    // Call .json() to parse the data. This returns a new Promise.
    return response.json(); 
  })
  // This .then() handles the second Promise (the actual data)
  .then(data => {
    console.log('Here is the data:', data);
  })
  // This .catch() handles any errors from the fetch or the .then() blocks
  .catch(error => {
    console.error('Fetch failed:', error);
  })
  .finally(() => 
    console.log("Done fetching") // final callback
  );      
```
- We just did **promise chaining**
	- Instead of calling the second `then()` inside the handler for the first `then()`, we return the promise returned by `json()` and call the second `then()` on that return value
	- we avoid callback hell
- Note on `response.ok` and `.catch()`
	- `response.ok`
		- Check for HTTP errors (like 404 or 500) *after the server has successfully sent a response*
		- Network connection is still successful -> just sees if [[HTTP Fundamentals#HTTP Request Methods|HTTP requests]] worked lol
	- `catch()`
		- Handle network failures or anything that prevents the request from completing
		- Gets *ANY* response back if it *just cannot connect to the server* (user is offline, server doesn't exist, fundamental network error, etc)
# Multiple promises (promise chain)
- Main functions include `.all()` and `.any()`, there are more ([Promise web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise))
## `Promise.all()`
- Takes an array of promises and returns a single promise
	- Sometimes you need promises to be fulfilled but they don't depend on each other
- The promise returned by `Promise.all()`
	- fulfilled when and if _all_ the promises in the array are fulfilled
		- the `then()` handler is called with an array of all the responses, in the same order that the promises were passed into `all()`
	- rejected when and if _any_ of the promises in the array are rejected (`catch()` handler is called)

```js
const fetchPromise1 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);
const fetchPromise2 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found",
);
const fetchPromise3 = fetch(
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json",
);

Promise.all([fetchPromise1, fetchPromise2, fetchPromise3])
  .then((responses) => {
    for (const response of responses) {
      console.log(`${response.url}: ${response.status}`);
    }
  })
  .catch((error) => {
    console.error(`Failed to fetch: ${error}`);
  });
```

## `Promise.any()`
- You might need any one of a set of promises to be fulfilled and don't care which one
- Like `Promise.all()` except that it's fulfilled as soon as *any* of the array of promises is fulfilled, or rejected if *all* of them are rejected

```js
const fetchPromise1 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);
const fetchPromise2 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found",
);
const fetchPromise3 = fetch(
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json",
);

Promise.any([fetchPromise1, fetchPromise2, fetchPromise3])
  .then((response) => {
    console.log(`${response.url}: ${response.status}`);
  })
  .catch((error) => {
    console.error(`Failed to fetch: ${error}`);
  });
```

# `async`/`await`
- A newer syntax that makes working with Promises much cleaner and easier to read
- Lets you write asynchronous code that *looks synchronous*
- `async`
	- Place this keyword before a function declaration: `async function myDataFunction() {...}`
	- An `async` function automatically returns a promise
	- Only inside an `async` function, you can use the `await` keyword before a call to a function that returns a promise
- `await`
	- Place the `await` keyword in front of any expression that returns a Promise (like `fetch()` or `response.json()`)
	- It **pauses the execution** of the `async` function *at that line* until the Promises settles (fulfilled or rejected)
		- If promise fulfilled -> `await` returns the fulfilled value
		- If promise rejected -> `await` throws an error
	- Because `await` throws an error if Promise is rejected, you **MUST** use a `try... catch` block to handle errors

```js
// You must declare the function as async
async function getData() {
  try {
    // Pause execution until fetch() completes and gives the Response
    const response = await fetch('https://api.example.com/data');

    // Check if the HTTP response was ok
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }

    // Pause execution until .json() completes and gives the data
    const data = await response.json();

    // This line only runs after the 'await' above is complete
    console.log('Here is the data:', data);

  } catch (error) {
    // Any error (network failure, failed .json() parse) is caught here
    console.error('Fetch failed:', error);
  }
}

// Call the async function to start it
getData();
```
- note that for `respose.json()`, `json()` also returns a promise so you must use `await`

