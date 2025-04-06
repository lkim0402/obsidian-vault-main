- `sorted(iterable, key=key, reverse=reverse)`
	- `iterable`: 	Required. The sequence to sort, list, dictionary, tuple etc.
	- `key`: Optional. 
		- A Function to execute to decide the order. Default is None
		- "How should we sort the data?" (ex. `len`)
		- allows you to specify a function that will be applied to each element in the iterable. The result of this function is then used to determine the order of the sorting.
	- `reverse`: Optional. A Boolean. False will sort ascending, True will sort descending. Default is False
- A built in function that will work with other iterable collections (ex. [[Tuples|tuple]])
```python
nums = [4,2,5,1]
sorted(nums) # [1,2,4,5], returns a copy of a list that is sorted
sorted(nums, reverse = True) # [5,4,2,1]
prints(nums) # [4,2,5,1]
```
- It returns a sorted copy, which is different from [[List#Methods specific to lists|the sort method for list]], which sorts the list in place
- You get a list no matter what type of iterable you put in

### Using it on a list of dicts
- We use [[Lambdas|lambda]]
	- We use lambda in sorted if there are properties in the object (like different keys in a dictionary) and we want to sort by that object
```python
users = [
	{"username": "samuel", "tweets": ["I love cake", "I love pie", "hello!"]},
	{"username": "katie", "tweets": ["I love my cat"]},
	{"username": "jeff", "tweets": [], "color": "purple"},
	{"username": "bob123", "tweets": [], "num": 10, "color": "teal"},
	{"username": "doggo_luvr", "tweets": ["dogs are the best", "I'm hungry"]},
	{"username": "guitar_gal", "tweets": []}
]


sorted(users) # ERROR
sorted(users, key=len) # number of keys in dict

# To sort users by each of their username alphabetically
sorted(users,key=lambda user: user['username'])

# Finding our most active users...
# Sort users by number of tweets, descending
sorted(users,key=lambda user: len(user["tweets"]), reverse=True)

# ANOTHER EXAMPLE DATA SET==================================
songs = [
	{"title": "happy birthday", "playcount": 1},
	{"title": "Survive", "playcount": 6},
	{"title": "YMCA", "playcount": 99},
	{"title": "Toxic", "playcount": 31}
]

# To sort songs by playcount
sorted(songs, key=lambda s: s['playcount'])

```