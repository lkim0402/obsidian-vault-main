- Definition
	- "There will be a variable"
	- `int x;`
- Assignment
	- "Put this value in the existing variable"
	- Giving value after definition (or initialization)
	- `int width;`
	- `width = 5;`
- Initialization
	- "here is the initial value for this thing"
	- Giving a first value AT THE SAME TIME as definition
	- `int x = 5;`
	- `int y{10};`

```cpp
int x;          
// DEFINITION: "an int named x"
// DEFAULT-INITIALIZATION: "no specific starting value"

int y = 5;      
// DEFINITION: "an int named y"
// COPY-INITIALIZATION: "starts with value 5"
```
#### Copy-Assignment
- Copies the value on the RHS of the `=` operator to the variable on the LHS of the operator
```cpp
#include <iostream>

int main()
{
	int width;
	width = 5; //copy assignment of value 5 into variable width
	std::cout << width; //prints 5

	width = 9; //changing value
	std::cout << width; //prints 9

	return 0;
}
```

### Initialization
> Best practice: Initialize your variables upon creation.
- Initialization: Definition + giving it a value
- Initializer: the syntax used to initialize an object
```cpp
int width { 5 }; // define variable width and initialize with initial value 5
// variable width now has value 5
```
- `{ 5 }` is the initializer, `5` is the initial value

#### Ways to initialize variables in C++
```cpp
// default-initialization (no initializer)
int a;         

// Traditional initialization forms:
int b = 5;     // copy-initialization (initial value after equals sign)
int c ( 6 );   // direct-initialization (initial value in parenthesis)

// Modern initialization forms (preferred):
int d { 7 };   // direct-list initialization (modern, preferred)
int d = { 7 }; // copy-list initialization
int e {};      // value-initialization (empty braces, will be 0)
```
- default-initialization 
	- `int a;`
	- In most cases, default-initialization performs no initialization, and leaves the variable with an indeterminate value
- copy-initialization
	- `int b = 5;`
	- Just like copy-assignment, it just copies the RHS to the LHS
- direct-initialization
	- `int width ( 5 );`
	- initially introduced to allow for more efficient initialization of complex objects (those with class types
	- fell out of favor due to direct-list-initialization
		- also since it's hard to distinguish from calling functions
		- `T(5);`
			- function call if `T` is a function
			- direct-initialization of temporary object if `T` is a type
- list-initialization
	- Also called  uniform initialization/brace initialization
	- Also can initialize objects with a list of values rather than a single value
	- direct-list-initialization (preferred)
		- `int d { 7 };`
	- copy-list initialization (rarely used)
		- `int d = { 7 };`
- value-initialization
	- `int width {};`
	- Most cases, it will initialize the variable to 0 (or empty), in which it is then called zero-initialization
	- Use it when the object's value is temporary and will be replaced;

### List initialization: narrowing conversions
> List-initialization is generally preferred over the other initialization forms because
> -  it works in most cases (and is therefore most consistent)
> - it disallows narrowing conversions (which we normally don’t want)
> - it supports initialization with a list of values

```cpp
int main()
{
	int w1 {4.5}; 
	// compiler error: list init does not allow narrrowing conversion of 4.5 to 4

	int w2 = 4.5; // compiles: copy-init initializes width with 4
	int w3 (4.5); // compiles: direct-init initializes width with 4

	return 0;
}
```

### Initializing multiple variables
```cpp
int a = 5, b = 6;          // copy-initialization
int c ( 7 ), d ( 8 );      // direct-initialization
int e { 9 }, f { 10 };     // direct-list-initialization
int i {}, j {};            // value-initialization
```

Common mistake
```cpp
int a, b = 5;     // wrong: a is not initialized to 5!
int a = 5, b = 5; // correct: a and b are initialized to 5
```
- `a` is left uninitialized
```cpp
int a = 4, b = 5; // correct: a and b both have initializers
int a, b = 5;     // wrong: a doesn't have its own initializer
```

### Unused initialized variables -> warnings!
- Modern compilers will typically generate warnings if a variable is initialized but not used (since this is rarely desirable).
```cpp
#include <iostream>

int main()
{
    int x { 5 };
    std::cout << x; // variable now used somewhere, no compile errors
    return 0;
}
```
##### The `[[maybe_unused]]` attribute C++17
- Going through the list of variables to remove/comment out the unused ones takes time and energy (and later going through uncommenting those is annoying too)
```cpp
#include <iostream>

int main()
{
	[[maybe_unused]] double pi {3.14};
	[[maybe_unused]] double gravity {9.8};
	[[maybe_unused]] double phi {1.61803};
	
	std::cout << pi << '\n';
	std::cout << phi << '\n';

	// compiler doesn't warn that gravity is not used
	return 0;
}
```
- The `[[maybe_unused]]` attribute should only be applied selectively to variables that have a specific and legitimate reason for being unused