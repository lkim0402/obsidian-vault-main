- There is a lambda for each value in the iterable
- Returns filter object which can be converted into other iterables
- **Object only contains the values that return true to the lambda!**
- Related: [[map function]]
	- Similar in a way that it makes use of [[Lambdas|lambdas]]

```python
nums = [1,2,3,4]
evens = list(filter(lambda x:x % 2 == 0, nums)) # [2,4]
```

```python
"""
Given this list of names
Return a new list with the string
"Your instructor is " + each value in the array,
but only if the value is less than 5 characters
"""
names = ['Lassie', 'Colt', 'Rusty']

# My approach
filtered = (filter(lambda x : len(x) < 5, names)) 
# no need to convert to list, <filter object at 0x7f8b5da44340>
ans = list(map(lambda x: f"Your instructor is {x}", filtered))
# ans = ['Your instructor is Colt']

# same ans but using list cmprehension
[f"Your instructor is {name}" for name in names if len(name) < 5]
```
- [[List Comprehension]] is better in this example.... but it's still important to know that functions like map, filter exist
	- But still if you can use list com, then you should lol