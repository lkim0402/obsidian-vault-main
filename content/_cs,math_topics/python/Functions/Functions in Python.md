> Stay DRY - Don't Repeat Yourself!
- Related: [[Scope]]

```python 
def square(int num):
	return num**2
```
- `return` 
	- exits the function
	- pops the function off the call stack 

### Parameters vs Arguments
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
**Keyword arguments**
- Arguments passed to a function using the parameter names (keywords) explicitly
- Useful when we pass in a [[Dictionary (Hashmap)|dictionary]] to a function and unpack its values
```python
def full_name(first, last):
	return f"Full name: {first} {last}"

full_name(last="Kim", first="Leejun")
```

### Exercises
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
