# Object destructuring
- You can extract properties from objects into distinct variables
- just access them with `.`
```js
const book = {
	id: 1,
	title: 'Tom Sawyer',
	genres: ["novel", "adventure", "fiction"]
}
// one method
const id = book.id;
const title = book.title;

// object deconstructuring
const {id, title, genres} = book 
const firstgenre = genres[0]

// Array Destructuring based on position 
const [primarygenre, secondgenre] = genres
```
- `{}` - object destructuring
	- you list as many properties to extract as you want
- `[]` - array destructuring

# Rest/Spread Operator
>Very important to remember with using [[React]]!!

With arrays
```js
// rest operator (getting the rest of the stuff)
const [primarygenre, secondgenre, ...otherGenres] = genres
```
- `otherGenres` is an array containing the rest of the operators
```js
// spread operator
const newGenres = [...genres, "drama"];
// ["novel", "adventure", "fiction", "drama"]
```

With objects
- what if we want to make a new object based on the current object but with a new property?
```js
const book = {
	id: 1,
	title: 'Tom Sawyer',
	genres: ["novel", "adventure", "fiction"]
}
const updatedBook = { ...book, moviePublicationDate: `2001` };
const updatedBook = { ...book, moviePublicationDate: `2001`, id: 2 };
```
- can also use this to override existing properties