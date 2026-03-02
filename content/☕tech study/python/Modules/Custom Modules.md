- You can import your own code too!
- Basically your own  file w/ python code that we can import to another file
- you import the **name of your python file (w/o py)**
- When is it good to make your own modules
	- code that needs to be used in more than one place
	- u have a rly long file and you can group some parts together

```python
# file1.py
def fn():
	return "something"

#file2.py
import file1 # name of the file
file1.fn() # "something"
```