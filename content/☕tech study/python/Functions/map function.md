- A standard function that accepts at least 2 arguments: **function** and an "**iterable**"
	- functions: usually [[Lambdas|lambda functions]]
	- iterables: list, strings, dictionaries, sets, tuples
- Runs the function/lambda for each value in the iterable and returns a map object which can be converted into another data structure
```python
nums = [1,2,3,4]
double = map(lambda x : x * 2, nums)
print(double) # <map object at 0x7f892b82e470> (a map object)
print(list(double)) # [2,4,6,8]

# same thing
double = list(map(lambda x : x * 2, nums))
```
- Can only be iterated once!! 
	- so if you make it a list again and print, its an empty list

### Examples
```python
#getting only first names in a list
names = [
	 { 'first':'Leejun','last':'Kim' },
	 { 'first':'Kirby','last':'Yoshi'}
]
# lambda -> for every name, return the first name only 
first_names = list(map(lambda x: x['first'],names))

# decrememting each number in list
def decrement_list(nums):
    return list(map(lambda x:x-1, nums)) # [0,1,2]
decrement_list([1,2,3])  
```
