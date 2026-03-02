- Literal / literal constant
	- A fixed value that has been inserted directly into the source code
	- literals have both value and type
```cpp
# "Hello World" and 5 (not x) are literals
std::count << "Hello World";
int x { 5 };
```

- Literal vs variable
```cpp
# literals vs variables
int main()
{
	# 5 is a literal that is printed
	std::cout << 5 << '\n';

	# x is a variable that is printed
	int x { 5 };
	std::count << x << '\n';
	
	return 0;
}
```
- Both output statements do the same thing (print the value 5). 
	- literal -> the value `5` can be printed directly
	- variable -> the value `5` must be fetched from the memory the variable represents

## Operators
- operation
	- process involving 0 or more input values (**operands**) that produces a new value through an **operator**
- operator
	- `+,-,\,*,=,>>,<<,==`
	- keywords such as `new`, `delete`, `throw`
	- Most operators in C++ just use their operands to calculate a return value(w/ exceptions such as `delete` and `throw`)
	- some has side effects
		- `std::cout<<5` has the side effect of printing `5` to console
- arity
	- number of operands that an operator takes as input is called the operator’s **arity**
	- unary
		- `operator-` takes operand `5` and outputs `-5`
	- binary
		- takes operands on left and right
		- for `4+5`, `operator+` takes `4` and `5`
		- `<<` (insertion) and `>>` (extraction) takes  `std::cout` or `std::cin` on the left side, and the value to output or variable to input to on the right side
	- ternary
		-  3 operands
		- conditional operator
	- nullary
		- 0 operands
		- `throw`
- Advanced stuff
	- For the operators that we call primarily for their return values (e.g. operator+ or operator*), it’s usually obvious what their return values will be (e.g. the sum or product of the operands).
	- For the operators we call primarily for their side effects (e.g. operator= or operator<<), it’s not always obvious what return values they produce (if any). For example, what return value would you expect x = 5 to have?
		- Both operator= and operator<< (when used to output values to the console) return their **left operand**. 
		- Thus, `x = 5` returns `x`, and `std::cout << 5` returns `std::cout`. This is done so that these operators can be chained.
		- For example, x = y = 5 evaluates as x = (y = 5). First y = 5 assigns 5 to y. This operation then returns y, which can then be assigned to x.
		- std::cout << "Hello " << "world!" evaluates as (std::cout << "Hello ") << "world!". This first prints "Hello " to the console. This operation returns std::cout, which can then be used to print "world!" to the console as well.