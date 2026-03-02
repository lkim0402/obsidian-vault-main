> - Initialized = The object is given a known value at the point of definition.
> - Assignment = The object is given a known value beyond the point of definition.
> - Uninitialized = The object has not been given a known value yet.

- Uninitialized
	- the object has not been given a known value yet (through any means, including assignment). 
	- So, an object that is not initialized but is then assigned a value is no longer _uninitialized_ (because it has been given a known value).
- Uninitialized variable
	- a variable that has not been given a known value (through initialization or assignment) 

`int x;`
- `x` is uninitialized
- In most cases when no initializer is provided, the variable is default-initialized, and performs no actual initialization
- it's a performance optimizationinherited from C when computers were slow

## Undefined behavior (UB)
> Using values of uninitialized variables can lead to unexpected results
> - result of executing code whose behavior is not well-defined by the C++ language

```cpp
#include <iostream>

int main()
{
	//define int variable x
	int x; // this variable is uninitialized because we haven't given it a value

	// print value of x to the screen
	std::cout << x <<'\n'; // who knows what we'll get!
	return 0;
}
```
- Computer will assign some unused memory to `x` ,  it will then send the value residing in that memory location to `std::cout`
- This may give issues
- A possible workaround
```cpp
#include <iostream>

void doNothing(int&) // Don't worry about what & is for now, we're just using it to trick the compiler into thinking variable x is used
{
}

int main()
{
    // define an integer variable named x
    int x; // this variable is uninitialized

    doNothing(x); // make the compiler think we're assigning a value to this variable

    // print the value of x to the screen (who knows what we'll get, because x is uninitialized)
    std::cout << x << '\n';

    return 0;
}
```
- Using the value from an uninitialized variable is our first example of undefined behavior (UB)
