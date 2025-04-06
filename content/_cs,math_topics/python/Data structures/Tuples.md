> An ordered collection or grouping of items
- It's **immutable**!!
	- Makes your code more safe because you can exist it to not change
- Tuples are faster than [[List|lists]]!
- Can use tuples as valid keys in a [[Dictionary (Hashmap)|dictionary]] 
- Data in tuple doesn't have to be unique
```python
nums = (1,2,3,4,3)
3 in nums # True

# Can access just like a list
nums[0] # 1
# CANNOT reassign elements
nums[0] = 7 # Error

# Using tuples as keys
locations = {
	 (30,120):"house",
	 (48.6, 92.5):"company"
}

# nested tuples
x = (1,2,3,(4,5),6)
```
### Tuple looping
```python
# same as list
names = ("colt", "blue")
for name in names:
	print(name)
```

### Methods
- count, index, slicing
```python
# count: Number of times a value appears in tuple
	x = (1,2,3,3,3,3,4)
x.count(2) # 1
x.count(3) # 4

# index: the index at which a value is found
x.index(2) # 1
x.index(3) # 2
x.index(3,3) # 3

# slices
x[-1] # 4
```

### Notes
```python
num = (range(0,3))  
print(num) #range(0, 3)  

num = tuple(range(0,3))  # MAKE SURE TO ADD THE KEYWORD 'TUPLE'
print(num) #(0,1,2)

num = tuple(num**2 if num % 2 == 0 else num + 1 for num in range(0,5))  
print(num) # (0, 2, 4, 4, 16)

"""Single element tuple"""
num = (1)
type(num) #int
num = tuple(1) # GIVES ERROR
num = (1,) # adds comma to distinguish it from the regular expression


"""list to tuple"""
a = [1,2,3]
tuple(a) # (1,2,3)

```