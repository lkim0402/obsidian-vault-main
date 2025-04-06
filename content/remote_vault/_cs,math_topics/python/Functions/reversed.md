- Returns a reverse iterator
- Different from the [[List# Methods specific to lists|reverse method from list]]
	- reverse = a list method, done in place
	- reversed = takes any iterable

```python
reversed([1,2,3,4])
# <list_reverseiterator object at 0x7f5210f10520>
list(reversed([1,2,3,4]))  
# [4,3,2,1]

# useful for other use cases other than list, like iterating over something in reverse  
for char in reversed("hello"):
	print(char)
```