- Every file/module has a `__name__` variable
- If the file is the **main file being run**, its value is "`__main__`"
	- if it's not, then its the file name
- When you use import, Python...
	- Tries to find the module (if it fails it throws error)
	- **Runs** the code in the module being imported!!
	- Then returns to the original file that is run and executes remaining code
- To ignore the code on import:
```python
# on the file that is being imported
# so that it only runs when its the main file being ran
if __name__ == "__main__":
# some code
```
