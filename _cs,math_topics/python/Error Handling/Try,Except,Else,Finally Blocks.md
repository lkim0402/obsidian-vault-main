- In python, it's **strongly** encouraged to use try/except blocks to catch exceptions when we can do something about them.

### try and except
- most commonly used
```python
try:
	foobar
except NameError as err:
	print(err) # name 'foobar' is not defined

def get(d,key):
    try:
        return d[key]
    except KeyError:
        return None

""" When we use an error that is not the error """
try:
	foobar
except ValueError as err:
	print(err)
print("after the try") # Doesn't get printed
#Traceback (most recent call last):  
# File "/home/lkim/Desktop/python_practice/error_handling.py"  
#, line 2, in <module>  
#   foobar  
#NameError: name 'foobar' is not defined

""" NOT RECOMMENDED """
try:
	foobar
except:
	print("ERROR")
# we're catching EVERY error, so we're not correctly identifying "what" went wrong
# catch all the errors. if there's more errors, add more except cases
```
### else and finally
- `else`
	- runs **only if no `exceptions` are raised**
	- if you `return` something in `try`, then `else` is never reached
- `finally` will run no matter what!
```python
try: 
	num = int(input("Enter a number: "))
except:
    print("Not a number!")
else:
    print("in else!")
finally:
    print("in finally!")

# Enter a number: 4  
# in the else!  
# in the finally!

# Enter a number: d  
# Not a number!  
# in the finally!
```

### A common structure: while loop
```python
# guessing game
while True:
	try:
		num = int(input("Number: "))
	except:
		print("Not a number!")
	else:
		print("Good job!")
		break
	finally:
		# this gets printed even after break in else
		print("PRINTED NO MATTER WHAT!") 
```