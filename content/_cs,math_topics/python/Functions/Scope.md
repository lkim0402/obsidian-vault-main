- [[Functions in Python]]
- Variables created in functions are scoped in that function

### `global` variable
- Variables not declared in a function
- accessible from any function within the same module or script
	
```python
"""Won't work!"""
total = 0
def increment():
	total == 1
	return total

increment() #ERRORRRRR

"""Works using global keyword"""
def increment():
		global total 
		total += 1
	return total

```
### `nonlocal` variable
- used in nested functions to indicate that a variable refers to a variable in the nearest enclosing scope that is not global
```python
def outer():
	count = 0
	def inner():
		nonlocal count <<<<<<
		count += 1
		return count
	return inner()
```
- `nonlocal` allows us to modify the count variable that was only locally available in the `outer()` function
- not used commonly but useful for imaging scope