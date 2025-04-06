> An object can take many (poly) forms (morph)

### Case 1
1. The same method in a class works in a similar way for different classes
	- Common implementation is to have a method in a base (or a parent) class that is overridden by a subclass -> **method overriding**
```python
class Animal():
	def speak(self):
		raise NotImplementedError("Subclass needs to override this!")

class Cat(Animal):
	def speak(self):
		return "meow"

class Dog(Animal):
	def speak(self):
		return "woof"

Cat.speak() # meow
Dog.speak() # woof
```

### Case 2
2. Special/dunder/magic methods
	- **Magic methods** are special methods that define how objects behave with Python's built-in operations (like `+`, `len()`, `==`, etc.).
	- They allow you to define behavior for built-in operations in Python, like arithmetic operations, comparisons, and more
		- Ex) when you use `+` to add two objects, Python internally calls the `__add__()` method on those objects
	- Same method behaves differently depending on which object(s) it's working with
```python
A = (1,2)
B = [1,2]
len(A)
len(B)
# A and B's data types are different, but len can be used!
```
- You can declare special methods on your own classes to mimic the behavior of builtin objects, such as `__len__` or `__add__`
```python
class Human:
	def __init__(self, height):
		self.height = height # in inches
		
	def __len__(self):
		return self.height
		
	def __repr__(self):
		return f"Human {self.name}"

Tim = Human(150)
len(Tim) # 150
print(Tim) # Human Tim
```