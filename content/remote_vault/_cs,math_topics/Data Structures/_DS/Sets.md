``myset = {"apple", "banana", "cherry"}
- do not allow duplicate values
- unchangeable
- **unordered** (inaccessible by indexing)
	- unlike [[Tuples|tuples]], which are ordered
- Once a set is created, you cannot change its items, but you can remove items and add new items.

```python

s = set()
s = set({1,4,5})
s = set({1,4,4,4,5,5,5,5}) # {1,4,5}
s = {1,4,5}

4 in s #True

for num in s:
	print(num)
```
- Useful cases
	- You have a list which has duplicate data in it
```python
cities = ["Seoul", "Seoul", "Seattle"]
print(set(cities)) # {'Seoul', 'Seattle'}
print(list(set(cities))) ['Seoul', 'Seattle']
```

### Methods
```python

my_set.add("new")
my_set.remove("new") #no error if not present
my_set.discard("new") #shows error if not presend
another_set = my_set.copy()
my_set.clear() #empties
```
### Set math
```python
# Union (combination)
first = {"colt", "leejun", "red"}
sec = {"leejun", "blue"}
first | sec # {'leejun', 'blue', 'colt', 'red'}

# Intersection (common elements)
first & sec # {'leejun'}
	
```
### Set comprehension
```python
{s**2 for s in range(4)}
# {0,1,4,9}

# checking if word has all vowels
def are_all_vowels_in_string(string):
	#if string = "hello", results in {'e','o'}
	s = {c for c in string if c in "aeiou"}
	return len(s) = 5
```