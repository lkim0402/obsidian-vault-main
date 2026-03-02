### OOP
> TODO: Add concepts from 331
1. What is Object-Oriented Programming (OOP)?
	- It is a method of programming that attempts to model some process or thing in the world as a class or object. A very nice way of thinking about structuring code.
2. What is a Class?
	- A blueprint for creating objects. It contains methods (functions) and attributes (similar to keys in a dictionary).
3. What is an Object?
	- An instance of a class. Objects are constructed from a class blueprint and contain their class's methods and properties.
4. What is Encapsulation?
	- The grouping of public and private attributes/methods into a programmatic class, making abstraction possible.
	- Key aspects:
		- Data hiding: Protects an object's data/internal state from outside, exposing only what's necessary.
		- Public interfaces: Functions that allow interaction with external code.
    - The goal is to expose the bare minimum and keep it "encapsulated."
5. What is Abstraction?
	- Exposing only the relevant data in a class interface while hiding the private attributes and methods (the "inner workings") from users.

### Python specific stuff
1. `__init__` method
	- It is the constructor of a class that defines how an object should be set up when created
	- Takes parameters for initializing instance attributes.
	- The first parameter is always `self`, referring to the instance being created.
```python
class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age

user1 = User("Alice", 19)
print(user1.name)  # Output: Alice
```
2. Instance Attributes and methods
	- **Instance Attributes:** Attributes specific to an instantiated object (e.g., `self.name`, `self.age`).
	- **Instance Methods:** Operate with a specific instance of the class, always taking `self` as the first parameter, allowing access to instance attributes.
```python
class User:
    def __init__(self, first, last, age):
	    # instance attributes
        self.first = first
        self.last = last
        self.age = age

	# instance method
    def full_name(self):
        return f"{self.first} {self.last}"

user1 = User("Kirby", "Kim", 0)
print(user1.full_name())  # Output: Kirby Kim
```
3. **What are Class Attributes?**  
	- Attributes shared by all instances of a class and the class itself. They are used less frequently than instance attributes. 
	- Common uses include:
		- Keeping track of how many instances have been created.
		- Default or shared data.
		- Constants.
	- Used with the class name
```python
class User:
    active_users = 0  # Class attribute

    def __init__(self, first, last, age):
        self.first = first
        self.last = last
        self.age = age
        User.active_users += 1  # Increment when a new user is instantiated

user1 = User("Kirby", "Kim", 0)
print(User.active_users)  # Output: 1
```
4. **What are Class Methods?**  
	- Methods that operate on the class itself, not on instances. They are often used for operations related to the class as a whole. 
	- Class methods use the `@classmethod` decorator.
```python
class Pet:
    total_pets = 0

    def __init__(self, name, species):
        self.name = name
        self.species = species
        Pet.total_pets += 1  # Increment total pets

    @classmethod
    def total_pet_num(cls):
        return f"Total pets: {cls.total_pets}"

print(Pet.total_pet_num())  # Output: Total pets: 0
```
5. What is the __repr__ method?
	- A method that provides a nice string representation of an object. It is called when you use the print() function or str() on an object.


How does Python know how to interpret the `+`  operator differently in these cases?
```python
5 + 5 = 10
"5" + "5" = "55"
```
- The 1st argument has a special method that defines what to do with the `+` operator
	- Python checks the type of the left operand.
	- It looks for the __add__() method in that operand's class.
	- If the left operand's class does not define __add__(), it checks the right operand's class, and if both classes have it, the method from the left operand's class is used.

#### Iterables and generators
- What is an **iterable** in Python?  
	- An **iterable** is an object that can return an iterator. It has an `__iter__()` method or defines a `__getitem__()` method that can take indexes. Examples include lists, strings, tuples, and files.
- What is an **iterator** in Python?  
	- An **iterator** is an object with state that keeps track of where it is during iteration. It implements the `__next__()` method to return the next item and raises `StopIteration` when there are no more items.
- What is the difference between an iterable and an iterator?  
	- An **iterable** is an object that can return an iterator, whereas an **iterator** is the object that actually performs the iteration, using the `__next__()` method to get each item.
- Can an object be both an iterable and an iterator?
	- Yes, an object can be both an iterable and an iterator if it implements both __iter__() and __next__() methods, as in the case of some custom iterator classes.