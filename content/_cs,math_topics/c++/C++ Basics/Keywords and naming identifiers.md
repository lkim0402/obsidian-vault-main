- keyword
	- C++ reserves a set of 92 keywords (as of C++23) for its own use and each has special meaning
		![[Pasted image 20241106172617.png]]
- special identifiers
	- C++ also defines : _override_, _final_, _import_, and _module_.

### Naming identifiers
- identifier: the name of a variable (or function, type, or other kind of item) is called an identifier. 
- naming rules
	- Can't be a keyword
	- Only be composed of letters (lower or upper case), numbers, and the underscore character. That means the name can not contain symbols (except the underscore) nor whitespace (spaces or tabs).
	- Must begin with a letter (lower or upper case) or an underscore. It can not start with a number.
	- C++ is case sensitive
- convention
	- starts lowercase
	- Identifier names that start with a capital letter are typically used for user-defined types
	- underscore or camel case (both conventional)

```cpp
# variables
int my_variable_name;
int myVariableName;
# functions
int my_function_name();
int myFunctionName();
```
- Tips
	- When working in an existing program, use the conventions of that program
	- identifier purpose should be clear, `int ccount` vs `int customerCount`