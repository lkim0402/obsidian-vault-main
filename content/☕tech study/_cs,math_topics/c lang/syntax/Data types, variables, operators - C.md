# Built-in Data types

| Data type | bytes |
| --------- | ----- |
| bool      | 1     |
| int       | 4     |
| float     | 4     |
| long      | 8     |
| double    | 8     |
| char      | 1     |
| string    | ?     |

- `int`
	- whole numbers
	- 4 bytes of memory (32 bits)
	- The range of values they can store is necessarily limited to 31 bits worth of information
	- $-2^{31} \text{ to } 2^{31} - 1$
	- - 2 billion to 2 billion
- `unsigned int`
	- `unsigned` is a *qualifier*
		- can be applied to certain types (including `int`
		- Effectively doubles the positive range of variables of that type at the cost of disallowing any negative values
	- $0 \text{ to } 2^{32} - 1$
	- 0 to 4 billion
- `char`
	- single characters
	- 1 byte of memory (8 bits)
	- We've developed a mapping of characters to numeric values using ASCII
	- $-2^7 \text{ to } 2^7 - 1$
	- -128 to 127 
- `float`
	- used for variables that will store floating-point values - real numbers
		- has decimal points
	- 4 bytes of memory (32 bits)
	- It has a precision problem
		- ex) look at $\pi$, it goes on forever - there is a limitation coz we only have 32 bits
	- No number line because it's hard to express the range of decimals
- `double`
	- variables that will store floating-point values, also known as real numbers
	- also stores `float`, but double precision
	- 8 bytes of memory (64 bits) - double the precision
		- additional 32 bits from float
- `string`
	- unknown because it depends on how long the word it
- `void`
	- is a *type* but not a *data type*
	- functions can have a void return type or a void parameter 

# Non-built-in
- In the context of CS50
- In C, `boolean` and `string` are NOT built in!
	- use the `#include <cs50.h>`

# Creating variables
- It's good to create variables when you need it
```c
// declaration
int number;
int height;

int height, number; // works, but bad practice

// assignment
number = 10;

// initialization = declaration + assignment
char letter = 'H';
```

```c
const int num = 0;
```
- `const` 
	- a constant when the value never changes
# Operators
- There are 2 types of boolean expressions: logical and relational operators
- Logical operators
	- `&&` : **Logical AND**. Returns true if both operands are true.
	- `||` : **Logical OR**. Returns true if at least one of the operands is true.
	- `!` : **Logical NOT**. Inverts the logical state of its operand (true becomes false, false becomes true).
- Relational operators
	- `==` : Equal to (**don't confuse with `=`**, which is for assignment)
	- `!=` : Not equal to
	- `>` : Greater than
	- `<` : Less than
	- `>=` : Greater than or equal to
	- `<=` : Less than or equal to

📌Truncation
```c
(1 + 2 + 3) / 3 // This just truncates
(1 + 2 + 3) / 3.0 // just use a float
(1 + 2 + 3) / (float) 3 // or cast it
```
## Other operators
- Assignment Operators =
	- These are used to assign values to variables.
	- `=` : Simple assignment (e.g., `x = 10;`)
	- `+=` : Add and assign (e.g., `x += 5;` is shorthand for `x = x + 5;`)
	- `-=` : Subtract and assign
	- `*=` : Multiply and assign
	- `/=` : Divide and assign
	- `%=` : Modulus and assign