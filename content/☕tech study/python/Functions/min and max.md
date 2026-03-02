- `max`: returns the largest item in an iterable or the largest of 2 or more arguments
- `min`: returns the smallest item in an iterable or the smallest of 2 or more arguments
```python
"""== max =="""
max(3,6,8) # 8
max([3,4,5]) # 5
max("hello world") # 'w'

"""== min =="""
min(1,3) # 1
min((1,3)) # 1
```

### Example
```python
names = ['Arya', 'Samson', 'Dora', 'Tim', 'Ollivander']

min(names) # 'Arya'

# the shortest length among the names
min(len(name) for name in names)


""" Using the key parameter """
# getting the longest name
max(names, key = lambda name:len(name))
# lambda will take each name and return len(name), and we'll use that to judge the max

```
