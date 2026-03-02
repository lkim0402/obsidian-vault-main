> Stay DRY - Don't Repeat Yourself!
- Related: [[Scope (global, nonlocal)]]

```python 
def square(num):
	return num**2
```
- `return` 
	- exits the function
	- pops the function off the call stack 

# Parameters vs Arguments
- Parameter
	- A variable in a method declaration
	- Parameters assigned in order (unless using keyword arguments)
- Arguments
	- When a method is called, the arguments are the data you pass into the method's parameters

Default parameters
```python
def exponent(num, power=1): #Power is defaulted to 1
	return num**power

# CAN BE OTHER FUNCTIONS!
def add(a,b):
	return a + b

def subtract(a,b):
    return a - b

def math(a,b,fn=add):
	# defaults to add function unless specified
	return fn(a,b) 

print(math(1,2)) # 3
print(math(1,2,subtract)) # -1
print(math(1,2,subtract())) # NEED TO SPECIFY FOR SUBTRACT FUNCTION!!
```
## **Keyword arguments**
- Arguments passed to a function using the parameter names (keywords) explicitly
- Useful when we pass in a [[Python Dictionary (Hashmap)|dictionary]] to a function and unpack its values
```python
def full_name(first, last):
	return f"Full name: {first} {last}"

full_name(last="Kim", first="Leejun")
```

# Type hinting
- Type hinting is primarily used for arguments (inputs), and secondarily for the return value (output).
- it's optional lol
```python
# SYNTAX
def function_name(parameter: Type) -> ReturnType:
    #...

# With Type Hint (Explicit)
def add(a: int, b: int) -> int:
    return a + b

# Without Type Hint (Implicit - totally fine)
def add(a, b):
    return a + b
```
- reasons to use type hinting:
	- activates better autocomplete (vs code, pycharm, etc)
	- documentation
	- error catching before running

- Optional arguments
```python
from typing import Optional

def find_user(user_id: int) -> Optional[dict]:
    # Returns a dict if found, or None if not found
    if user_id == 1:
        return {"name": "Alice"}
    return None
```

- The python interpretor literally ignores your type hints at runtime!!-> It is only external tools (linters) that will complain
```python
def get_number() -> int:
    return "I am a string lol"  # <--- This is valid Python code!

x = get_number()
print(x) # Output: "I am a string lol"
```
# Exercises
```python
"""Checking if num is odd"""
def is_odd_number(num):
	return True if num % 2 == 1 else False

def is_odd_number(num):
	if num % 2 == 1:
		return True
	return False

"""counting $ signs"""
def count_dollar_signs(word):
    # count = 0
    # for char in word:
    #     if char == '$':
    #         count += 1
    return sum(1 for c in word if c=="$")
    
""" char count in string"""
def multiple_letter_count(s):
    ans = {}
    for char in s:
        ans[char] = 1 + ans.get(char,0)
    
    return ans

# using count
def multiple_letter_count(s):
	return {char:s.count() for char in s}
```
