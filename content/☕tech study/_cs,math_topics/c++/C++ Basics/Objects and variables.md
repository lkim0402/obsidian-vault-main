- data
	- any information that can be moved, processed, or stored by a computer
	- the information that the (computer) program works with to produce a result
- value
	- a single piece of data
	- `a`, 5, `Hello`
- RAM - Random Access Memory
	- When we run a program, the program and any data hardcoded into the program itself (text such as `Hello World!`) is loaded at this point
	- The OS also reserves some additional RAM for the program to use while running, commonly to store values entered by the user/reading data from somewhere/storing values while the program runs so they can be used again later!
	- Direct memory access is discouraged in C++, so we access memory indirectly through an **object**
		- Related: [[PC#RAM (Memory)|RAM section in PC]]
- object
	- represents a region of storage (typically RAM or CPU register) that can hold a value
	- used to store a value in memory
	- we focus on using objects to store and retrieve values, and not worry about where in memory those objects are actually being placed
		- KEY POINT - you don't need to worry about *where exactly it's stored*
	- Objects can be unnamed in C++, but we mostly name objects using an identifier - an object with a name is called a variable
		- identifier - the name that a variable is accessed by
- variable (named object)
	- an object with a name (identifier)
	- naming our objects let us refer to them again later in the program
	- *subset of objects*

https://www.reddit.com/r/cpp_questions/comments/1gk0u7f/difference_between_object_and_variables_in_c/

| Objects                                                                      | Variables (named object)                                                                |
| ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Includes all data in memory<br>(`int x = 5 + 3`,  5, 3, `x` are all objects) | Only represent the named objects in memory<br>(`int x = 5 + 3`,  only`x` is a variable) |
| Can be unnamed or unnamed                                                    | named                                                                                   |

### Variable instantiation
> [!Variables]
> - Every variable has a **type**, which represents the kind of information you can store inside of it. 
> - It tells your compiler how much memory to set aside for the variable, and it defines what you can do with the variable.
> - "Every variable in C++ must be declared before it can be used!"

- Defining a variable - **Definition**
	- Defining a variable named `x` of type int
		- `int x;`
- At compile time (program is being compiled)
	- (blueprint)
	- the compiler makes note of variable of name `x` and type `int`
	- whenever the compiler sees the identifier `x`, it will know we're referencing this variable
	- "I want a space that can hold an integer, call it x"
	- What compile time does
		- Check for syntax errors
		- Make notes about variables/functions
		- Create a plan for memory usage
		- Turn your code into machine instructions (executable file)
- At runtime (program is run)
	- (actually using the blueprint to built)
	- variable is created (instantiated) when their code is reached during run time
	- **Instantiation**
		- a specific storage location (RAM/CPU) register) has been allocated for the object's use
		- An instantiated object = An instance

- At runtime, **an object must be instantiated before it can be used to store a value**. 
```c++
int x;           //1. Space created for x (at location 140) - instantiation
x = 5;           //2. Storing value at x (location 140) after it's instantiated
std::count << x; //3. Program looks at x's value to get 5
x = 10;          //4. Location 140 now stores 10
```
- For the sake of example, let’s say that variable x is instantiated at memory location 140. Whenever the program uses variable x, it will access the value in memory location 140. An instantiated object is sometimes called an instance.

### `Const`
- `const` variables cannot be changed by your program during execution
- `const double quarter = 0.25;`

### Type Conversion
- the notation `(type) value` means change `value`to `type`
```c++
double a;
int b;

a = 10.9;
b = (int) a;

//going from double to int just removes the decimal; no rounding!
```
### Compile time vs Runtime!!!!
```cpp
// COMPILE TIME - ALL code is analyzed
void sayHello() {   // Compiler: "Ok, there will be a function called sayHello"
    std::string msg = "Hello";  
    // Compiler: "Inside sayHello, there will be a string called msg"
}

int main() {      // Compiler: "Ok, there will be the main function"
    int x;        // Compiler: "Inside main, there will be an int called x"
    sayHello();   // Compiler: "Ok, there will be a call to sayHello"
    return 0;     // Compiler: "Got it, main will return 0"
}

// RUNTIME - Program executes, starting with main()
// 1. main() starts
// 2. Space for x is created (instantiation)
// 3. sayHello() is called
//    - Space for msg is created (instantiation)
//    - Copies the value "Hello" into that space (initialization)
//    - sayHello() finishes, msg is destroyed
// 4. main() returns 0
```

During compile time, the compiler makes plan for memory allocation, then at runtime it actually allocates the memory
- Defining variables happen at compile time
- Memory is allocated at runtime (actual space is created)
```cpp
// ======== Compile time ========

int x;          // Compiler: "I need to prepare for 8 bytes of space"
double y;       // Compiler: "I need to prepare for 4 bytes of space"
char z = 'A'    // Compiler: "Plan: need 1 byte for a char" 
                // "Also note: it should be initialized with 'A'"

// Compiler creates executable with instructions like: 
// 1. "Allocate 4 bytes for x" 
// 2. "Allocate 8 bytes for y" 
// 3. "Allocate 1 byte for z and put 'A' in it"

// ======== Runtime ========
int x;          // Computer: "Following instructions..."
                // "Ok, found and allocated 4 bytes for x"

double y;       // "Found and allocated 8 bytes for y"

char z = 'A';   // "Found and allocated 1 byte for z"
                // "Putting 'A' in that space"
```
- That's why compilation can succeed even if your computer doesn't have enough memory to run the program - the actual memory allocation doesn't happen until runtime!

### Defining multiple variables
```cpp
// case 1
int a;
int b;

// case 2 (same with case 1)
int a, b;

// ====== WRONG WAYS ======
int a, int b; 
int a, double b; // defining variables of different types in the same statement

// fixing
int a; int b; // correct but not recommended

// correct & recommended
int a;
int b;

```