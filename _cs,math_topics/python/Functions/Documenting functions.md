- Use """ (msg) """
- Essential when writing complex functions
```python
def say_hello():
	# docstring
	"""A simple function that returns the string hello"""
	return "Hello!"
	
say_hello.__doc__ # 'A simple function that returns the string hello'
help(say_hello)
```
- `__doc__`
	- returns the docstring of a function