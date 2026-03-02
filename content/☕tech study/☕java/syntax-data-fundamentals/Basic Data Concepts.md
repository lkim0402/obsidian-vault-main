
>[!Data type]
>A name for a category of data values that are all related, as in type `int` in Java, which is used to represent integer values.

- Java is a *type-safe* language, meaning you have to be explicit about what kind of information you intend to manipulate.
- [[Primitive vs Reference types]]
# Expressions & Evaluation
- Expression
	- A simple value or a set of operations that produces a value
	- Can be arbitrarily complex with as many operators as you like
	- Example
		- literal values: `42`, `28.9`
		- complex expressions (using operators): `3 + (18 * 7)`
			- operators: `+`, `*`
			- operands: `3`, `18`, `7`
- Evaluation
	- The process of obtaining the value of an expression => a *result*
# Operators
## Arithmetic operators

| Operator | Name               | Description                                                                                                                                   | Example (`int x = 10, y = 4;`)                                           |
| -------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| `+`      | **Addition**       | Adds two values together.                                                                                                                     | `x + y` evaluates to `14`                                                |
| `-`      | **Subtraction**    | Subtracts the right-hand operand from the left-hand operand.                                                                                  | `x - y` evaluates to `6`                                                 |
| `*`      | **Multiplication** | Multiplies two values.                                                                                                                        | `x * y` evaluates to `40`                                                |
| `/`      | **Division**       | Divides the left-hand operand by the right-hand operand. **Note:** If both are integers, the result is an integer (the remainder is dropped). | `x / y` evaluates to `2` (not `2.5`)                                     |
| `%`      | **Modulus**        | Returns the remainder of an integer division.                                                                                                 | `x % y` evaluates to `2` (since `10 / 4` is `2` with a remainder of `2`) |
- In division, if one operand is a `double` then the result is always evaluated to a `double` 📌
- Casting
	- `(int) 4.8` => `4` (simply truncates)
	- It just casts the value right after it, so use parenthesis to be careful: `(int) (23.5/23)`
## Unary Operators

| Operator | Name          | Description                        | Example (`int count = 5;`)       |
| -------- | ------------- | ---------------------------------- | -------------------------------- |
| `++`     | **Increment** | Increases a variable's value by 1. | `count++;` // `count` is now `6` |
| `--`     | **Decrement** | Decreases a variable's value by 1. | `count--;` // `count` is now `4` |
- Position matters
	- Post-increment (`count++`): The variable's _original_ value is used in the expression first, and _then_ it's incremented.
	- Pre-increment (`++count`): The variable is incremented _first_, and the _new_ value is used in the expression.
## Compound Assignment Operators

|Operator|Example|Equivalent To|
|---|---|---|
|`+=`|`a += b`|`a = a + b`|
|`-=`|`a -= b`|`a = a - b`|
|`*=`|`a *= b`|`a = a * b`|
|`/=`|`a /= b`|`a = a / b`|
|`%=`|`a %= b`|`a = a % b`|
# Precedence
>[!Overview]
>The binding power of an operator, which determines how to group parts of an expression.

- The computer applies rules of precedence when the grouping of operators in an expression is ambiguous
- Multiplicative operators (`*`, `/`, `%`) have a higher level of precedence than the additive `+`,`-` 
	- same level of precedence is evaluated from left to right
- Override precedence with `()`

# Conversion
```java
// ❌ Not Allowed (Narrowing conversion without cast)
double x = 3;
int y = x; // java doesn't allow this because converting to a lower rank data type might result in data loss

// ✅ Allowed (Widening conversion)
int x = 3;
double y = x;

// ✅ Forced Conversion (Casting)
double x = 3.14;
int y = (int) x;
```
# JShell
>[!Overview]
>A program with a prompt where you can type arbitrary Java expressions.
>- Once you press Enter, the expression you typed will immediately be evaluated and its result will be displayed

- It's a *REPL (Read-Evaluate-Print Loop)* tool
	- A program that prompts the user for an individual code expression, then executes the expression and displays its result immediately
	- Useful when you're learning to program or learning a new language because you can try out commands quickly and get instant feedback
- The JDE now includes it starting from Java 9
- *Consider launching JShell to test new concepts!*
- Commands
	- In terminal, just type `jshell`
	-  `/exit` -  stop
	- `/vars` - shows all variables u declared
		- IDs like `$1, $2` are automatically generated if you don't give a variable name to a given expression (you can use if you want to refer to a previous computation)
# Literals
>[!Overview]
>The simplest expressions refer to values directly using what are known as *literals*.
- [[Primitive vs Reference types]]
- 📌They are different from primitive types! 
	- A **literal** is the **actual fixed value** that you write in your code to represent a specific primitive value (or sometimes a `String`)
	- Every literal has a type, often one of the primitive data types.
	- Primitive type = a category of values (eg. `int`)
	- Literal = a specific constant value of that category (eg. `5`)
## Primitive type literal
- An integer literal (type `int`)
	- A sequence of digits with or without a leading sign
	- 3, 475, -235, 0 +235
- A floating-point literal (type `double`)
	- Includes a decimal point
	- 3252.9, 0.2, -32.1
	- Can also be expressed in scientific notation: `2.3e5`, `1e-5`, etc
		- `2.3e5` = `2.3 * 10^4` 
		- `1e-5` = `1 * 10^{-5}`
		- Even though the values evaluate to an integer, it's still a type `double` because it's expressed in scientific notation
- A character literal
	- Enclosed in single quotation marks and includes just one character
	- \`a\`, \`b\`, \`!\`, \`\\\\\`
	- \`\\\\\` -> you use an escape sequence
- A boolean literal
	- Stores logical information
	- `true`, `false`
## String literal
- Explained in this note: [[String (Literal & Class)]]
## Null literal
- Represents the absence of a value or the non-existence of an object
- Allows you to create a reference type and point it to `null` instead of an object
- Can be used for all reference types but not for any primitive types ([[Primitive vs Reference types]])
- You can declare variables with it, but you cannot use these variables until you reassign a suitable, non-null value
# Variables
>[!Overview]
>A memory location with a name and a type that stores a value

- Need to explicitly tell what *type of data* you are going to store & the name to use to refer to this memory location
- Standard convention is to start variable names with a lowercase letter & capitalize subsequent words (camel)
- Assignment operator evaluates from right to left
```java
int x, y, z;
x = y = z = 2 * 5 + 4;
x = (y = (z = 2 * 5 + 4));
```
## Variable declaration
- A request to set aside a new variable with a given name and type
- `<type> <name>;`
	- `double height;`
- *Uninitialized variables*
	- Since there's no value to put in the memory yet, Java doesn't store initial values in these memory locations 
	- Like blank cells in a spreadsheet
- You can declare several variables that are all of the same type in a single statement
	- `<type> <name>, <name>, <name>, ..., <name>;`
	- `double height, weight;`
## Assignment statement
- After a variable declaration, we can get values into those cells in memory using assignment statement
- `<variable> = <expression>;`
	- `height = 70;`
	- This statement stores the value 70 in the memory location for the variable `height`
	- Because it's a simple literal value, the computer just puts the value in the memory location
## Declaration + Assignment
- `<type> <name> = <expression>;`
	- `double height = 70;`
- You can declare + initialize at them at the same time
	- `double height = 70, weight = 195;`
## Local variable type inference
- Even though Java is statically typed, since Java 10, you can declare a variable's type as `var` and let Java compiler automatically infer the type
	- Other statically typed languages like C++ also support type inference
- It only works with local variables 
	- [[Local VS Class VS Instance variables]]