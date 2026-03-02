#### Built-in functions
```c++
#include <iostream>
#include <cmath>

int main() {
  
  // This seeds the random number generator:
  srand (time(NULL));
  
  // Use rand() below to initialize the_amazing_random_number
  int the_amazing_random_number = rand() % 29;
  
  std::cout << the_amazing_random_number <<std::endl;
}
```

#### Custom functions
```c++
# returning void
void matke_sandwich()
{
	std::cout << "bread\n";
	std::cout << "egg\n";
	std::cout << "veggies\n";
	std::cout << "bread\n;
}

# returning values
std::string feed_the_cat() {
  return "Cat is fed!";
}

int main()
{
	std::string cat_message = feed_the_cat();  
	std::cout << cat_message;
}
```

#### Where should I declare my functions?
- For small programs, it's a question of consistency and style. You want to be able to find the main quickly, so either always put it at the top or always put it at the bottom -- just be consistent about it.
- Once you start building larger programs, you'll be moving to using multiple files. Then you'll want to put the declarations in header files (.h) and the definitions in their own file, grouping like functions together, such as done with string routines (strcmp, strcpy) can be found in string.h and math routines (cos, sin, tan) are in math.h
	- [source](https://www.reddit.com/r/C_Programming/comments/fk9621/where_should_i_declare_my_functions/)

### Scope
- Variables defined in global scope are accessible throughout the program.
- Variables defined in a function have local scope and are only accessible inside the function.

### Modularity
- To make your code cleaner and more modular, you can move the function definitions over to another specialized `.cpp` file (e.g., `my_functions.cpp`), leaving a list of declarations above `main()`.
	- List in C++ is a sequential container that stores elements in non-contiguous memory locations, allowing for efficient insertion and deletion at any position.
- One step compiling and linking
	- `g++ main.cpp my_functions.cpp`
		- Compiles `main.cpp` and `my_functions.cpp` into object files (`main.o`, `my_functions.o`).
		- Links the object files into a single executable.
	- The output is an executable, named `a.out` by default or specified with the -o flag.

```c++
// main.cpp
#include <iostream>
void greet(); // Declaration

int main() {
    greet(); // Call
    return 0;
}
```

```c++
// my_functions.cpp
#include <iostream>
void greet() { // Definition
    std::cout << "Hello, World!" << std::endl;
}
```
- `g++ main.cpp my_functions.cpp` gives one executable `a.out`

### Header
- Instead of putting declarations, you can take those function declarations and move them all over to a _header file_ (so you won't have to repeat for all different files)
	- has extension `.hpp` or `.h`
	- `#include "my_functions.hpp"`

### Inline functions
An _inline function_ is a function definition, usually in a header file, qualified by `inline` like this:
```c++
inline 
void eat() {
  std::cout << "nom nom\n";
}
```
- Advises the compiler to insert the function's body where the function's call is
- Sometimes helps with execution speed (or hinders)
- Prop you'll encounter but never use

Another inline function
- member functions (functions in classes) which have been defined and declared in a single line in a header file because the function body is so short
```c++
// cookie_functions.hpp (header file)

// eat() belongs to the Cookie class:
void Cookie::eat() {std::cout << "nom nom\n";} 
```
- You should ALWAYS add the inline keyword if you are inlining functions in a header (unless you are dealing with member functions, which are automatically inlined for you).

### Default arguments
- You could add default arguments to function declarations
	- Default arguments = values assigned to parameters then the function is declared and defined
```c++
// Declaration
void intro(std::string name, std::string lang = "C++");

// Definition
void intro(std::string name, std::string lang) {
  std::cout << "Hi, my name is "
            << name
            << " and I'm learning "
            << lang
            << ".\n";
}

intro("Mariel")
// "Hi, my name is Mariel and I'm learning C++."

intro("Mariel", "Python")
// "Hi, my name is Mariel and I'm learning Python."

```
- Then, if you leave the argument blank in your function call, instead of an error, your function will run with the default value. 
- Call where the first argument IS default but the second is NOT:
	- `greet( , 25);` // This is NOT valid in C++. You cannot skip the first argument (name) while trying to override the second (age).
	- `greet("Friend", 25);`

### Function overload
>[!How it works]
>In a process known as function overloading, you can give multiple C++ functions the same name. Just make sure at least one of these conditions is true:
>- Each has different type parameters.
>- Each has a different number of parameters.

Enables you to change the way a function behaves depending on what is passed in as argument
```cpp
void print_cat_ears(char let) {
  std::cout << " " << let << "   " << let << " " << "\n";
  std::cout << let << let << let << " " << let << let << let << "\n";
}

void print_cat_ears(int num) {
  std::cout << " " << num << "   " << num << " " << "\n";
  std::cout << num << num << num << " " << num << num << num << "\n";
}

```

### Function Templates
>[!What is it]
>Templates in C++ are a **powerful feature** that allow you to write generic and reusable code.
>- A template is a blueprint for creating functions or classes with **placeholder types**. Instead of specifying an exact type (e.g., `int` or `float`), you use a **type parameter** (like `T`). The compiler will replace `T` with the actual type when the function or class is used.
>- They enable you to write functions or classes that can operate with any **data type** without having to rewrite the same logic for each type.
- Overloading can get tedious
- This feature comes in handy for classes as well as for functions. 
	- `std::string` and `std::vector` are both template-based types
	- The C++ standard library is full of templates
	- All created in header files!!

#### W/o templates
```cpp
int add(int a, int b) {
    return a + b;
}

float add(float a, float b) {
    return a + b;
}

// Must write for every type!
```

#### W/ templates
```cpp
//inside a header file

template <typename T>
void print_cat_ears(T item) {

  std::cout << " " << item << "   " << item << " " << "\n";
  std::cout << item << item << item << " " << item << item << item << "\n";

}

// in main.cpp
print_cat_ears(2);

// the output:
//  2   2
// 222 222

```
- we can use `print_cat_ears()` with a parameter of any type we want, as long as the type can be used with the methods expected