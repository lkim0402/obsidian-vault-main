# Named `export`
## Using `export` on everything
```js
export const studentsArr = [
	{
		name: 'Mark',
		age: "29",
		score: "4.0"
	},
	{
		name: 'Jane',
		age: "27",
		score: "3.8"
	}
]

// in another file
import { studentsArr as students} from "./test/test";
console.log(studentsArr[0].name);
```
- `studentsArr as students`
	- giving it an alias
- You can add in more `import`s if they're from the same file
	- `import {studentsArr as students, teacherArr}`
## Using `export` just once & include
- Instead of adding `export` to every `const`, just use it once
```js
const studentsArr = [
	{
		name: 'Mark',
		age: "21",
		score: "4.0"
	},
	{
		name: 'Jane',
		age: "22",
		score: "3.8"
	}
]

const teachersArr = [
	{
		name: 'Mary',
		age: "29",
	},
	{
		name: 'John',
		age: "27",
	}
]
export {studentsArr, teachersArr}
```
# Using `default export`
- You can only have **one default export per file**, but that one thing can be a function, a class, variable, object, array
- You can contain other exports, but only one default export
- When you `import` it, you can call the function whatever you want!
	- This can be both good and bad
```js
// function
export default function greet() {
  console.log("Hello!");
}

// class
export default class Person {
  constructor(name) {
    this.name = name;
  }
}

// variable
const config = { theme: "dark", lang: "en" };
export default config;

// object
export default {
  name: "John",
  age: 25,
  greet() {
    console.log("Hi!");
  }
};

// array
export default [1, 2, 3, 4, 5];
```

You can also just do it separately
```js
function getMatchingFilter(arr, keyword){
 ...
}
export default getMatchingFilter;

```

- **Default export**: One per file, imported without `{}`
- **Named exports**: As many as you want, imported with `{}`