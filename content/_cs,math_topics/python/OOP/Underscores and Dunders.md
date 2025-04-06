- `__name__`: dunder methods
	- We use dunder methods when we're overriding something that exists in python, something that python looks for
	- `__init__`
	- Reserved for special methods that have predefined behaviors and are part of python's core functionality
	- just don't use it
- `_name`: 1 underscore
	- indicates a private property
	- `self._secret`
	- OFC, nothing is really "private" in Python, so it's just a convention
- `__name` : 2 underscores
	- `self.__message`
	- This is **name mangling**!
		- changing the names of class attributes that start with a double underscore `__`
		- change the attribute name to something that is less likely to conflict with subclasses
		- It makes the attribute unique to the specific class
```python
class Person:
	def __init__(self):
		self.name = "Tony"
		self._secret = "hi"
		self.__msg = "I like turtles!"
p = Person()

print(p.name)
print(p.__msg) ## DOESN'T PRINT
print(dir(p))
# here, you sill see '_Person__msg' along with other methods
# so you have to do like this
print(p._Person_msg) # this prints "I like turtles!"
```
- single underscore, class name, double underscore, and the attribute