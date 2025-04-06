> Custom error messages!!
- [[Most type of common errors]]

```python
# raise <Error>(<message>)
raise ValueError('invalid value')
raise ValueError # just returns ValueError
```

Our own module for example`
```python
def colorize(text,color):
	colors = ("red","blue","purple","pink")
	# it's good to be clear - 1 error for every case
	if type(text) is not str:
		raise TypeError('text must be instance of str')
	if type(color) is not str:
		raise TypeError('color must be instance of str')
	if color not in colors:
		raise ValueError('invalid color')
	
	print(f"Printed {text} in {color}")
```