- Related: [[Python#OOP (Object Oriented Programming)|Python's OOP]]
- Objects
	- instance of a class that encapsulates data and functionality pertaining to that data
- Class definitions are usually placed in [[C++ Functions#Header|header files]]
### Classes
- user-defined types
#### Class members
- Components of a class
- Can be `public` or `private`
- Two types
	- **Member Variables/data**: data attributes (fields) of the class
	- **Member Functions (Methods)**: functions that you can use with an instance of the class
- Unless we have a mostly empty class, it’s common to split function declarations from definitions
	- In header: declare methods in the class
	- In cpp (outside the class): define methods
```cpp
// ================ city.hpp ================
//definitions
class City {
	int population;

	public:
		int get_population();
}

// ================ city.cpp ================

#include "city.hpp"

int City::get_population()
{
	return population
}
```
- you can access `population` within `city.cpp` even though it was declared within `city.hpp`!!
- `public` makes it accessible
	- every member is private by default

### Creating objects (instantiation)
```cpp
City seoul;
seoul.population = 200;
seoul.get_population();
```

### Constructors
-  a way to give an object some data right when it gets created
- use it when you want to instantiate an object with specific attributes
- no return type

```cpp
// ========== city.hpp ==========  
#include "city.hpp"
class City
{
	std::string name;
	std::int population;

	public:
		City(std::string new_name, int new_pop);
	
}

// ========== city.cpp ==========

// constructor for the City Class
City::City(std::string new_name, int new_pop)
	: name(new_name), population(new_pop) {}

// ========== inside main() ==========
City ankara("Ankara", 5445000);
```
- we can use parameters and a member initializer list to initialize attributes to values passed in members
	- initializer list: `name(new_name), population(new_pop)`
	- The initializer list is executed **before** the constructor body
	- used to initialize member variables directly, which can be more efficient, especially for **const** or **reference** members, which must be initialized this way
- initializer list > Assignment in Constructor Body
	- First default-initializes the members and then assigns new values to them.

### Destructors
> [!What it is]
> A _destructor_ is a special method that handles object destruction. 
> - Isreally about tidying up and preventing memory leaks. 
> - Like a constructor, it has the same name as the class and no return type, but is preceded by a `~` operator and takes no parameters
> - automatically invoked when an object is destroyed.

```cpp
//========== city.hpp ==========
class City
{

	std::string name;
	int population;

	public:
		City(std::string new_name, int new_pop);
		~City();
}

// ========== city.cpp ==========
City::~City()
{
	// any final cleanup
}

// in main.cpp
int main()
{
	City seoul("Seoul", 100);
	// Destructor will be called automatically when obj goes out of scope
	return 0;
}
```
- You generally won’t need to call a destructor; the destructor will be called automatically in any of the following scenarios:
	- The object moves out of scope.
	- The object is explicitly deleted.
	- When the program ends.
