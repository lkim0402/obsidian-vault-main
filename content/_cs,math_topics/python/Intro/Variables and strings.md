### Naming restrictions
- should be assigned before they can be used
- variables have to start with a letter or underscore
- the rest should be letters, numbers, or underscores
- case sensitive
- Conventions
	- **lower_snake_case** (underscores bet. words)
	- variables should be lower case (except constants)
	- UpperCamelCase usually refers to a class
	- Variables that start and end with two underscores are supposed to be private or left alone
- Double vs string quotes
	- Doesn't matter but just be consistent throughout the same file
	- Just a stylistic thing
- `None`
	- special value `None`
	- represent nothingness (python's version of `null`)
```python

# None leuwprd
a = None
type(child) # NoneType
```
### Escape characters/sequences
```python
new_line = "hello \nworld"
print(new_line)
#hello
#world

s = "backlast : \\"
print(s) #"backslash: \"

s = "I said \"ha ha\"" #just use ' inside or vise versa
print(s) # "I said "ha ha""
```
### String concatenation
- Need to be working with strings
```python
print("h" + "i") # "hi"
str1 = "ice"
str1 += " cream" #str1 is "ice cream"
```
### String formatting
- **String interpolation**: inserting variables, expressions, or values into a string
- f strings
	- `print("f{sample_variabele}")`
```python
greeting = "hi"
name = "heisenberg"
greet_name = f"{greeting} {name}"
```
### Strings and Indexes
- String is an iterable, and you can access individual chars with indexes
	- Like [[List|lists]] and [[Tuples|tuples]]
```python
name = "Chuk"
name[0] # "C"
```