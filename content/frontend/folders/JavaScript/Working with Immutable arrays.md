- in react, many operations need to be immutable
	- so these techniques are absolutely essential for [[== React ==|react development]]
- we're learning how to add/delete/update elements without touching the original array
	- always make new array
	- these would normally be abstracted into some functions

```js
// books is just an object that contains book objects
//1. Adding book
const newBook = {
  id: 6,
  title: "harry potter",
  author: "jk",
};
const booksAfterAdd = [...books, newBook];

// 2.Deleting book
const booksAfterDelete = booksAfterAdd.filter((book) => books.includes(book));

// 3. updating book object in the array
const booksAfterUpdate = booksAfterDelete.map((book) =>
  book.id == 5 ? { ...book, pages: book.pages + 100 } : book
);
```