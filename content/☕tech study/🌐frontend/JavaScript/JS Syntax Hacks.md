# Ternaries
(condition) ? (if) : (else)
```js
console.log(pages > 1000 ? "over 1000" : "less than 1000");
```

# Short Circuiting
- in some conditions the operator will immediately return the first value and not even look at the 2nd value
- falsy values: `0, '', null, undefined`

&& operator
- if 1st value is falsy, immediately returns the 1st value
```js
const hasBook = true;
const hasMovie = false;

console.log(true && "Some string"); //"Some string"
console.log(false && "Some string"); // false
console.log(hasBook && "This has a book") //"This has a book"
console.log(hasMovie && "This has a movie") //false

console.log("hello" && "some string") //"some string"
console.log(0 && "some string")// 0
```

|| and ?? operator
- if 1st value is truthy, immediately returns the 1st value
```js
console.log(true || "hello"); //true
console.log(false || "hello"); //"hello"

// can be used to set default values!!
const spanishTranslation = book.translations.spanish || "NOT TRANSLATED";
```
- be careful if the data is 0, this might not work as expected

??
- you can use the nullish coalescing operator to solve this
```js
const count = book.reviews.reviewsCount ?? "no data";
```
- returns the 2nd value when the 1st is `null` or `undefined`!! (so not when it's 0 or empty string)

# Optional Chaining
```js

function getTotalReviewCount(book) {
  const goodreads = book.reviews.goodreads.reviewsCount; //49701
  const librarything = book.reviews.librarything?.reviewsCount ?? 0; //0
  return goodreads + librarything;
}

console.log(getTotalReviewCount(book));
```
- `book.reviews.librarything?.reviewsCount ?? 0`
	- `?` stops if the property before it is `null` or `undefined` and returns `undefined` (doesn't throw errors)
	- because it's`undefined ?? 0`, the entire expression returns 0