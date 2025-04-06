- To build any meaningful react apps, you need to be a true master of these functional array methods in JS:
- functional array methods 
	- they do not mutate the original array, but returns a new array based on the original one
	- **map, filter, reduce**
	- slice, concat, flat
- Imperative (Mutating) Array Methods
	- These methods **mutate the original array** instead of returning a new one:
	- sort
	- foreach
# map
- loops over an array and returns an array with the same length with some operation applied
```js
const a = [1, 2, 3, 4, 5];
b = a.map((el) => el + 2); //[3,4,5,6,7]
```
- `(el) => el + 2` is a callback function that gets called for each of the elements in the array
- `el` parameter is always the current element of the array

```js
books = [
	{
		id: 1, 
		title: "Dune",
	    publicationDate: "1965-01-01",
	},
	...
]
// create an array that contains all the titles of all books
const titles = books.map((book) => book.title);

// if you want each element to be a JSON, wrap it with ()
const titles = books.map((el) => ({title: el.title, author: el.author}));
```
# filter
> we can filter the elements in an array based on some condition
```js
const longBooks = books.filter((book) => book.pages > 500);
```
- will contain book objects that has more than 500 pages
- `(book) => book.pages > 500`
	- this callback function will be called for each element of the array
	- if the condition is true, it will go in the array
- we can filter many times
```js
const longBooks = books.filter((book) => book.pages > 500).filter((book) => book.hasMovieAdaptation);
```
- books that have more than 500 pages AND has movie adaptation
- can just do an &&
	- `const longBooks = books.filter((book) => book.pages > 500 && book.hasMovieAdaptation`
);
# reduce
> Goal is to reduce the entire array to 1 value!
- most versatile and powerful
- 1st arg: a callback function which will be executed for each element in the array
	- `(acc, book) => acc + book.pages`
- 2nd arg: a starter value
	- `0`
```js
const pagesAll = books.reduce((acc, book) => acc + book.pages, 0);
```
- in the callback function
	- 1st arg - `acc`
		- the accumulator, the current value of the final value we want the array to boil down to
		- 0 is passed in as the second argument to `reduce()`, which is this acc
	- 2nd arg - `book`
		- the **current element being iterated over** in the array
- If you **omit the starter value**, the first array element becomes the starting value

Reduce manually
- [2626. Array Reduce Transformation](https://leetcode.com/problems/array-reduce-transformation/description/?envType=study-plan-v2&envId=30-days-of-javascript)
```js
/**
 * @param {number[]} nums
 * @param {Function} fn
 * @param {number} init
 * @return {number}
 */
var reduce = function(nums, fn, init) {
    if (nums.length == 0) {return init}

    acc = init
    for (let i = 0; i < nums.length; i++) 
    {
        acc = fn(acc, nums[i])
    }

    return acc
};
```
# sort
> sorts an array (IN PLACE). so its not a functional method!!!
- so we usually copy the array first using `slice()`
```js
const arr = [3,7,1,9,6];
const sorted1 = arr.slice().sort((a,b) => a - b); // ascending order
const sorted2 = arr.slice().sort((a,b) => b - a); // descending order

// sorting objects by # of pages
const sortedByPages = books.slice().sort((a, b) => b.pages - a.pages);
```
- `a,b` are the current value and the next value
- `a - b` 
	- A negative value when `a` is smaller than `b` (so `a` comes first).
	- A positive value when `a` is greater than `b` (so `b` comes first).
	- `0` if `a` is equal to `b`.
- `b - a`
	- A negative value when `b` is smaller than `a` (so `b` comes first).
	- A positive value when `b` is greater than `a` (so `a` comes first).
	- `0` if `a` is equal to `b`.

# foreach
- iterates over an array and executes a function on each element
- DOES NOT return a new array (unlike map)
```js
array.forEach((element, index, array) => {
    // Code to execute for each element
});
```
- `element`: The current element in the array.
- `index` (optional): The index of the current element.
- `array` (optional): The entire array being iterated over.

```js
/**
 * @param {number[]} arr
 * @param {Function} fn
 * @return {number[]}
 */
var map = function(arr, fn) {
    const ans = []

    arr.forEach((el, i) => {
        ans.push(fn(el, i))
    })
    return ans
};
```