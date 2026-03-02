- Computer program
	- A sequence of instructions. 
- Statements
	- A type of instruction that causes the computer program to perform some action
	- Most (but not all statements) end with `;`
	- Most common type of instruction in C++ programs, since they are the smallest independent unit of computation in C++
	- There are many different kinds of statements in C++:
		- Declaration statements
		- Jump statements
		- Expression statements
		- Compound statements
		- Selection statements (conditionals)
		- Iteration statements (loops)
		- Try blocks
- Function
	- A collection of statements that get executed sequentially (in order, from top to bottom). 
	- Every C++ program must have a special function named `main()`
	- In programming, the name of a function (or object, type, template, etc…) is called its ***identifier***
- Syntax error
	- A compiler error that occurs at compile-time when your program violates the grammar rules of the C++ language
### Hello World
```C++
//main.cpp
#include <iostream> //preprocessor directive

int main()
{
	std::cout << "Hello world!";
	# return 0; 
	# if you don't return anything it assumes you're returning 0 anyway
}
```
- Preprocessor directive
	- `#include` preprocessor directive indicates that we would like to use the contents of the `iostream` library, which is part of the C++ standard library that allows us to read/write text from/to the console
	- `#include <iostream>`
		- Gets evaluated before we compile the file
		- includes all contents of the iostream file and paste into our file `main.cpp`

