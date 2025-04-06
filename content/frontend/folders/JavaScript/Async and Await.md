> Basically syntactic sugar for promises.... the handlers will go away!
- only works for functions that return promises
- related
	- [[Promises]], [[== JavaScript ==|JS]]
### `async` function
- functions that ALWAYS returns a promise
- need keyword `await`
### `await`
- "WAIT for the promise to resolve!"
- tells JS to stop execution of the current function until a promise resolves, then return a Promise
- endless loop which checks if the promise has been resolved
- only works inside `async` functions

### Example
One use case WITHOUT the keywords `async` and `await`:
```JS
function sumAsync(x, y) {
    return new Promise((resolve, reject) => {
	    // sleep is a function that returns a promise
        sleep(500).then(() => {
            resolve(x + y);
        });
    });
}

// let's use the function now
sumAsync(5, 7).then((result) => {
    console.log("The result of the addition is:", result);
});

function sleep(ms) {
    return new Promise((resolve) => setTimeout(resolve, ms));
}

```

A better example using `async`, using `await` on the sleep function:
```JS
async function sumAsync(x,y)
{
	// waits for 500 ms
	await sleep(500);
	// you can chain more asynchronous functions
	// and NOT use infinite .thens!
	
	//returns value after done waiting
	return x + y;
}

// sumAsync is an async function because it returns a promise
sumAsync(5,6).then((result) => {
	console.log("Result of addition: ", result);
})

function sleep(ms) {
	return new Promise((resolve) => setTimeout(resolve, ms));
}
```
- `SumAsync` **implicitly** returns a `Promise`

### Example 2
- converting example from [[Promises#We need to `return` when chaining!|Promise w/ chain example]] to using `async` and `await`
```JS
// ------------------------ ORIGINAL ------------------------ 
function setup()
{
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
}

// ------------------------ MODIFIED ------------------------
function setup()
{
	wordGIF()
	.then(results => {
		createP(results.word);
		createImg(results.img_url);
		})
	.catch(err => {
		console.error(err);
	})
}
async function wordGIF()
{
	// no need to fetch & call .then!!
	// get all needed variables here
	let response1 = await fetch(wordnikAPIURL);
	let json1 = await response1.json();
	let word =  json1.word; 
	
	let response2 = await fetch(giphyAPI + word);
	let json2 = await response2.json();
	let img_url = json2.data[0].images['fixed_height_small'].url;
	// no await needed for non-Promises -> word and img_url

	return {
		word: word,
		img_url: img_url
	}
}
```

You can catch errors using the try catch

What if you want to print after u receive the data
```js
// WRONG!!! -----------
async function getTodos() {
  const res = await fetch("https://jsonplaceholder.typicode.com/todos");
  const data = await res.json();
  return data;
}
const todos = getTodos();
todos;

console.log(todos);

// CORRECT SOLUTION!!! -----------
async function getTodos() {
  const res = await fetch("https://jsonplaceholder.typicode.com/todos");
  const data = await res.json();
  return data;
}

async function printTodos(){
    const todos = await getTodos();
    console.log(todos);
}

printTodos();



```