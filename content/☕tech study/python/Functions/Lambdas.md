### WTF are lambda functions
- used commonly with:
	- [[filter function]]
	- [[map function]]
```python
def square(num):
	return num * num

def square(num): return num * num

print(square(9)) # 81

"""
A lambda function that does the same thing
lambda (some_parameter) : (a single expression that is automatically returned)
"""
lambda num: num * num # think of it like a no named function
square2 = lambda num: num * num # you can store it in a variable
print(square2(9)) # calling this lambda function

add = lambda a,b: a + b
print(add(1,2)) # 3
```

- Common use case: when you pass in a function to another function **as a parameter** and that function will never be used again
- The point is that it's in line and we don't need to make another function

```python
# a simple button using tk 
button = tk.Button(
   frame, 
   text="CLICK ME", 
   fg="red", 
   command=lambda: print("Hello") 
   # prints "Hello" whenever button is clicked
   # we don't even need a parameter
)
```