- `main` 
	- Usually a special function that is automatically executed when an operating system starts to run a program, like [[☕Java|java]]
	- But in Python you don't need to create a function named `main`, you can just have a file with just code & it will run it from top to bottom
- `__name__`
	- Every Python file has a special variable `__name__` 
	- When you run the file directly (ex. `python my_file.py`) in the terminal - Python sets `__name__ = "__main__"`
	- When you import the file as a module, if you have another file `importer.py` that says `import my_file`, Python sets `__name__ = "my_file"` inside `my_file.py`
- sources
	- https://realpython.com/python-main-function/

```python
def main():
    print("Hello World!")

if __name__ == "__main__":
    main()
```
- `if __name__ == "__main__":`
	- When the `if` statement evaluates to `True`, the Python interpreter executes `main()`.
	- So basically it's only set like that when we execute this file directly & not as a module
	- you can use this to distinguish when you run the [[Python Namespace, Modules, Packages#Modules|module]] as a script or after an `import`
		- Ex. Only print things when it's run in a script (add print statements under this condition)
- This code pattern is quite common in Python files that you want to be **executed as a script** and **imported in another module**
- No matter how you're running your code, Python defines a special variable named `__name__` that contains a string whose value depends on how the code is being used

