- `open()` function
	- built in function
	- you read a file by putting in the file name
	- primarily designed for text-based and binary files
		- text files: txt, csv, json
		- binary files: .png, .jpg
		- can open any file type, but it does not inherently understand how to interpret the content of complex formats like .pdf or .epub (you need a library)
	- `open()` then returns a ***file object*** to you
		- the file object not only contains the data but also the metadata and all other info 
- `read()` function
	- You read a ***file object*** with the `read()`
### `open()` and `read()` and `seek(num)`
```txt
This short story is really short!
```
```python
file = open("story.txt")
file
# <_io.TextIOWrapper name = 'story.txt' mode='r' encoding='UTF-8'>
# file is an instance of an TextIOWrapper class
# mode = 'r' -> we're reading it!

file.read() 
# gives me entire file
# "This short story is really short!"
file.read() 
# now empty!!
# ""
```
- **`TextIOWrapper`** 
	- a class that wraps around a file and provides functionality to read/write **text** data (strings) with encoding
	- The object created from this class has methods like `.read()`, `.write()`, `.close()`, etc., which you use to interact with the file.
- Cursor movement
	- Python reads files using a cursor (file pointer)
	- So after a file is read, the cursor is at the end!
		- If you `file.read()` again, then the cursor is alr at the end. So it doesn't have anything to return!
```python
content = file.read()  # Reads the entire file
print(content)

file.seek(0)  # Move the cursor back to the start of the file
print(file.read())  # Reads the file again from the beginning
```

```sh
>>> file = open("story.txt")
>>> file.read()
"This is a super short story!\nNow it's a little longer.\n"
>>> file.read()
'Hi!\n'
```
- You don't have to open the file again to get the new changes!!
- If you add `Hi!\n` to the end after `file.read()` and do `file.read()` again, then the cursor will now reflect it!!

### `readline()`
```txt
Hello! 
This is a random text.
Goodbye!
```
```python
file.readline() # Hello! 
# Now, file.read() will point to the 2nd line
file.read()
# This is a random text.
# Goodbye!
	
""" another example """
file = open("story.txt")

print(f"readline(): {file.readline()}")
print(f"readline(): {file.readline()}")
print(f"readline(): {file.readline()}")
print(f"readline(): {file.readline()}")

# lkim@fedora:~/Desktop/python_practice$ python file.py 
# readline(): Hello! 
# readline(): This is a random text.
# readline(): Goodbye!
# readline(): 
```
### `readlines()`
- gives you all lines but preserves them as separate lines in a list instead of putting them in a giant string
```python
file = open("story.txt")
print(f"readlines(): {file.readlines()}")
# readlines(): ['Hello! \n', 'This is a random text.\n', 'Goodbye!\n']
print(f"readlines(): {file.readlines()[0]}")
# readlines(): Hello! 
```

### `close()`
- When we `open()` a file, the changes made in that file will be connected to our code.
- so we close files (if we're done using it we manually close them so we don't use system resources)
```python
file = open("story.txt")
print(f"readlines(): {file.readlines()[0]}") 
# readlines(): Hello! 
file.close()
print(file.closed) # True
```

## How most ppl actually approach file io
### `with`
```python
with open('story.txt', 'r') as file:
	# Whatever code we want
	file.read()
	# file closed automatically after code block

file.closed # True
```
- **`open()`** returns a file object, which acts as a context manager.
- The **`with`** statement ensures that the file is opened. As soon as the `with` statement starts, the `__enter__()` method is called (means file is open)
	- if you just call `f.__enter__()` it will just return the file object
- Once the block is finished (whether by successful completion or due to an exception), the context manager's **`__exit__()`** method is automatically called, closing the file.
- So we can  make our own objects/classes which has `__enter__()` and `__exit__()`