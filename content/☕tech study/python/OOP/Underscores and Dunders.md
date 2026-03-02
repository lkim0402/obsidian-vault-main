- dunder methods
	- stands for "Double UNDERscore"
	- We use dunder methods when we're overriding something that exists in python, something that python looks for
		- ex) `__name__`: , `__init__`
	- Reserved for special methods that have predefined behaviors and are part of python's core functionality
- underscore properties
	- indicates a private property
		- ex) `self._secret`
	- OFC, nothing is really "private" in Python, so it's just a convention
- `__name` : 2 underscores
	- `self.__message`
	- This is **name mangling**!
		- changing the names of class attributes that start with a double underscore `__`
		- change the attribute name to something that is less likely to conflict with subclasses
		- It makes the attribute unique to the specific class
# example - `Vector` class
```python
class Vector:
    # 1. Constructor: Called when creating an instance
    def __init__(self, x, y):
        self.x = x
        self.y = y

    # 2. String Rep: Called by print() or str()
    def __str__(self):
        return f"Vector({self.x}, {self.y})"

    # 3. Arithmetic: Called by the + operator
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    # 4. Equality: Called by the == operator
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

# Usage
v1 = Vector(2, 4)
v2 = Vector(1, 1)

print(v1)       # Output: Vector(2, 4)   (Triggers __str__)
v3 = v1 + v2    # Output: Vector(3, 5)   (Triggers __add__)
print(v1 == v2) # Output: False          (Triggers __eq__)
```
- single underscore, class name, double underscore, and the attribute