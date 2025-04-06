- They're nice for testing stuff
- you can just loop over the iterable and check manually
### all
- Takes an iterable as an argument
- returns `True` is all elements of the iterable are truthy (or if the iterable is empty)

```python
all([0,1,2,3]) # False

# You can check if the items in an iterable meets a certain condition
# checking if all names start with "C"
people = ["Charlie", "Chuck", "Cody"]
all([name[0].upper="C" for name in people]) 
# list is [True,True,True], so all returns True

#checking is all nums are even
nums = [2,3,6]
all([num % 2 == 0 for num in nums]) # False
all(num % 2 == 0 for num in nums) # False (don't need [], it's a generator)

# checking if all iterms are string
def is_all_strings(iterable):
    return all(type(item)==str for item in iterable)
```
### any
- returns `True` if **any** element of the iterable is truthy
- returns `False` if the iterable is empty
```python
any([0,1,2,3]) # True
```

