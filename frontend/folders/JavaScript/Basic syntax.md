### Variables
```JavaScript
console.log("Hello, World");

// variables - can reassign
let name = "Leejun";
let age = 22;

// constants - can't reassign
const PI = 3.14;

console.log(name);
console.log(age);
```
- `var` is not recommended

Two special types called `undefined` and `null`
```JavaScript
const newVar;
console.log(newVar); // undefined

const emptyVar = null;
console.log(emptyVar); // null
```

### Operators
```JS
// +: addition bet. numbers AND string concatenation
var name = "John";
console.log("Hello " + name + "!");
console.log(1 + "1");   // outputs "11"

// == and ===
console.log("1" == 1); // true - same content
console.log("1" === 1); // false - same content & type
```

### Array 
```JS
var myArray = [1,2,3];
var myArray = new Array(1,2,3);
console.log(myArray[0]) // 1

// Arrays are sparse
var myArray = []
myArray[1] = 2
console.log(myArray) // [undefined, 2, undefined, undefined]

// can put diff. elems in array
var myArray = ["string", 3, true]

// ================ functionality ================
// array can function as stack
var myStack = [];
myStack.push(1);
myStack.push(2);
myStack.push(3);
console.log(myStack); // 1,2,3

console.log(myStack.pop()); // 3
console.log(myStack); // 1,2

// shift pops, but from the FRONT (so like a queue)
var myStack = [1,2,3]
console.log(myStack.shift()); // 1
console.log(myStack) // 2,3
// unshift inserts a variable in the beginning
myStack.unshift(1);
console.log(myStack); // 1,2,3

// SPLICING
// from index a, splice 5 elements
var myArray = [0,1,2,3,4,5,6,7,8,9];
var splice = myArray.splice(3,5);

console.log(splice); // 3,4,5,6,7
console.log(myArray); // 0,1,2,8,9

```

### Loops
```JS
var myArr = [1,2,3];
for (var i = 0; i < myArr.length; i++)
{
	console.log(i + " is " + myArr[i]);
}

// continue: skips the rest of the loop
for (var i = 0; i < 100; i++)
{
    // check that the number is even
    if (i % 2 == 0)
    {
         continue;
    }
    // if we got here, then i is odd.
    console.log(i + " is an odd number.");
}
```

### Object
- used as a data structure
```JS
const myObj = {}
const personObj = {
	first: "John", // use : and NOT =
	last: "Doe"
}

// 2 ways to address
personObj.age = 24;
personObj["age"] = 24;

// using hasOwnProperty when looping over objects to check if member is in object (as opposed to inheritance)
for (var member in personObj)
{
	if (personObj.hasOwnProperty(member))
	{
		console.log(member + " exists!");
	}
}
```

