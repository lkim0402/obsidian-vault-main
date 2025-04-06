- Useful for asynchronous programming in [[== JavaScript ==|javascript]]

> [!Promise]
> - The object that will **eventually** be returned by the function.
> - All asynchronous functions return a promise.
> - 2 traits
> 	- Receives a single argument which is a function
> 		- The function has 2 arguments, a `resolve` function and a `reject` function
> 	- The code written inside the promise needs to use one of these two functions
> - Can be waited on using `then` method (and other similar methods), or the `await` statement

### States
1. Pending (waiting to get the data back from the API)
2. Fulfilled (successfully resolved)
3. Rejected (error)
### `fetch()`
- A function where you give a URL of an API, then it "fetches" the stuff from the URL
- Returns a promise
- native to JS in the browser
- Convert it to an object that we can use -> `response.json()`
### `then(), catch()`
> Functions that you can call to the returned promise object, takes an argument
- `then()`
	- Receives a function to be executed when promise is fulfilled
	- allows you to do other things asynchronously while promise is not yet fulfilled!
	- each `.then()` returns a promise
- `catch()`
	- Receives a function to be executed when promise is rejected 

### `resolve(), reject()`
- functions you need to use when creating your *own* promise
- automatically provided to you by JavaScript when you create a new `Promise`
- `resolve()`
	- This function is called when the asynchronous operation inside the Promise succeeds, and it **fulfills** the Promise.
    - The result passed to `resolve` is what the **Promise** will resolve to (i.e., the value you get when the Promise is successful).
- `reject()`
	- This function is called when the asynchronous operation inside the Promise fails, and it **rejects** the Promise.
	- The argument passed to `reject` is usually an **error** or a message explaining why the operation failed.
- The names don't really matter (`resolve` or `reject`) but the ORDERING matters
	- You can have `new Promise((apple, banana))`

### Introductory Example
```JS
function getServerStatus()
{
	const result = fetch("/server/status");

	// this will NOT work!
	console.log("Status from the server: ", result.ok);
}
```
- `fetch` function returns a `Promise`
	- The fetch function is asynchronous, and it returns a promise. `console.log` will not work.
- This is the right approach - using `then`:
```JS
function getServerStatus()
{
	const response = fetch("https://jsonplaceholder.typicode.com/todos")
	  .then((res) => res.json()) 
	  .then((data) => console.log("Status from the server: ", data))
	  .catch((error) => console.log("Error fetching data: ", error));
}
```
- `then` function is one of the methods of a `Promise`
- `.then((res) => res.json()) `
	- **Reads the response body** as a JSON object.
	- **Returns a Promise** that resolves with the parsed JSON data

### Example 1
> Making your own promise
```JS
// adding 2 numbers
function sumAsync(x,y)
{
	console.log("1. sumAsync is executed!");

	const p = new Promise((resolve,reject) => {
		setTimeout(() => {
			console.log("4. Resolving sumAsync's Promise after 500ms");
			resolve(x+y);
			},  
			500
		);
		// no need to return anything
		console.log("2. sunAsync Promise is initialized");
	});
	console.log("3. sunAsync has returned the Promise");
	return p;
}

// actually using the function
sumAsync(5,7).then((result) => {
	console.log("5. The result of the addition is:", result);
});
```
- When something is done successfully, call `resolve` with the result 
- When you use resolve(x + y), you're calling the resolve function that JavaScript gives you to indicate that the Promise should resolve successfully with the result of x + y.
	- If there were an error and you wanted to reject the Promise, you'd explicitly code the reject in there

A shorthand:
```JS
// 1
new Promise((resolve, reject) => {
    setTimeout(resolve, 1000);  
    // Just calls resolve() after 1 second
	// shorthand way to call resolve() w/ no arguments after time passes!
});

//2
// Same as:
new Promise((resolve, reject) => {
    setTimeout(() => resolve(), 1000);  
});

//3
// Same as:
new Promise((resolve, reject) => {
    setTimeout(() => resolve(undefined), 1000);
});
```
1. `resolve`
	- You're passing a reference to the function so it can be called later
2. `() => resolve()`
	- This is creating an **anonymous arrow function** that will be invoked immediately by `setTimeout` after 1 second, and inside that anonymous function, you're calling `resolve()`.
	- `setTimeout(resolve(), 1000);` is wrong because you would be calling `resolve()` immediately and not passing it as a reference to `setTimeout`. 
		- `setTimeout` expects a function as the first argument and not the result of the function!
3. `() => resolve(undefined)`
	- `undefined` is directly being passed. it is passed by default, we're just explicitly stating that
### Example 2
- Adding more error catching
```JS
// adding 2 numbers
function sumAsync(x,y)
{
	const p = new Promise((resolve,reject) => {
		setTimeout(() => {
			if (x < 0 || y < 0)
			{
				reject("Only positives allowed!");
			}
			else
			{
				resolve(x+y);
			}
		}, 500);
	});
	return p;
}

// actually using the function
sumAsync(5,7).then((result) => {
	console.log("The result of the addition is:", result);
}).catch((error) => {
	console.log("Error: ", error);
})
```

### Example 3
Chaining!
```JS
// =========== ORIGINAL ===========
function setup()
{

	let promise = fetch("some/url");
	promise.then(gotData);
	promise.catch(gotErr);

	function gotData(response)
	{
		console.log("Success: ", data);
	}
	
	function gotErr(error)
	{
		console.log("Error: ", error);
	}
}

// =========== SHORTENED ===========
// you can chain stuff together
function setup()
{
	fetch("some/url")
		.then(response => console.log("Success: ", response))
		.catch(error =>  console.log("Error: ", error));
}

// WE NEED TO CONVERT IT TO JSON using .json, which also returns a promise
function setup()
{
	fetch("some/url")
		.then(response => data.json())
		.then(jsonResponse => console.log("Returned json: ", json))
		.catch(error =>  console.log("Error: ", error));
}
```

### We need to `return` when chaining!
```JS
fetch(wordnikAPIURL)
    .then(response => {
        return response.json();
    })
    .then(json => {
        createP(json.word);
        return fetch(giphyAPI + json.word);
    })
    .then(response => {
        return response.json();
    })
    .then(json => {
        createImg(json.data[0].images['fixed_height_small'].url);
    })
    .catch(err => console.log(err));
```
- Need to return in order for the next chain to use it
- Each `.then()` automatically wraps its return value in a new Promise!!!


### Promise.all()
- requires an array
```JS
let promises = [___, ___, ___];

Promise.all(promises)
	.then((result) => {
		for (let i = 0; i < results.length; i++)
		{
			createP(results[i].word);
			createImg(results[i].img);
		}
	})
	.catch((err) => {})
```
- when all these promises are complete and resolved, give me the results of those promises in an array in the same order as the original array
- all or nothing (you get the results at once or u get an error)
	- so use a try catch
### Resources
- Coding train!!!!
	- https://www.youtube.com/playlist?list=PLRqwX-V7Uu6bKLPQvPRNNE65kBL62mVfx