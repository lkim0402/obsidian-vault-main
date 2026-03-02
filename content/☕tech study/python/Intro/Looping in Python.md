# For loops
```python
for item in iterable_object:
	...

for char in "hello":
	print("char:" + char)

for num in range(1,8): #(1-7)
	print(num)
```

- `iterable_object`
	- some kind of collection of items
	- [[List (python)]], range
- `item`
	- a new variable that can be called whatever you want
	- references the current position of our iterator within the iterable
## Using `_`
You can use `_` to indicate that the loop variable is not used within the loop's body
```python
for _ in range(5):
    print("Hello!")
```
# While loop
```python
count = 0
while count < 5:
    print(f"Count is: {count}")
    count += 1 # Increment count to eventually make the condition False
print("Loop finished.")
```
- Just a standard while loop lol
# `range`
- Generates an ordered sequence of numbers 
- immutable, NOT A LIST, but a special iterable object
- commonly used in for loops
```python
# one number (end)
range(7) # 0-6

# two numbers (start, end)
range(0,7) # 0 - 6

# three numbers (start, end, step)
range(1,11,2) # 1,3,5,7,9
range(6,0,-1) # 6,5,4,3,2,1

# range to list
r = range(0,6) # 0,1,2,3,4,5
list(r) # [0,1,2,3,4,5]
```
- 