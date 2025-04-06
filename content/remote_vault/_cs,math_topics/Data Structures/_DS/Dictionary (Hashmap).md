> A dictionary is a collection of key-value pairs
- It cannot have more than one occurrence of the same key. Each key in a dictionary must be unique.
- NO specific order
- [[Dictionary comprehension]]
- [[**kwargs parameter]]

```python
my_dict = {}

my_dict = dict(key1 = "value1") # {'key1' = 'value1'}
my_dict = {'key1' = 'value1'} 

# Add a new key-value pair 
my_dict["key2"] = "value2"
my_dict = {"key1": "value1", "key2": "value2"} 

# Length of dict = # of keys!
length = len(my_dict)

# Merging 2 dicts
dict1 = {"key1": "value1"}
dict2 = {"key2": "value2"}
merged_dict = {**dict1, **dict2}  
```

```python
"""All return iterables"""

#Gets keys
my_dict.keys() # dict_keys(["key1","key2","key3"])

#Gets values
my_dict.values() # dict_values(["value1", "value2", "value3"])
list(my_dict.values()) # ["value1", "value2", "value3"]

#Gets items
my_dict.items() 
# dict_items([("key1","value1"), ("key2": "value2")])
# list of tuples
```

### Dictionary methods
- Built in dictionaries
```python

# Removes all key-value pairs from the dict
my_dict.clear()  


# Creates a shallow copy of the dict
copy_dict = my_dict.copy()  


# Creates key-value pairs from comma separated values:
{}.fromkeys("a",1) # {"a":1}
# If the 1st arg is an iterable, it will assign each item with the value
player = {}.fromkeys(["email","name"], 'unknown') # {'email':'unknown', 'name':'unknown'}
			test = {}.fromkeys("hi", 1) # {'h':1, 'i':1} (put it in a list)
# Returns new dict
player.fromkeys(['email'],'unknown') #{'email', 'unknown'}


value = my_dict.get(key)
value = my_dict.get(None_existing_key) # None (not error!)
value = my_dict.get(key, default_value)
# default_value: The value to return if the key is not found, else returns the value
```

- deleting a key-value pair
```python
# Removes the key-value pair with key 'key1' 
del my_dict["key1"] 
# Removes the key 'key1' and returns its value
removed_value = my_dict.pop("key1") 
# Removing a RANDOM KEY (no args)
removed_random = my_dict.popitem() # ('key1', 'value1)
```

- adding items from another dict
```python
first = {"a":1,"b":2,"c":3}
second = {"d":4}

second.update(first)
second #{"d":4,"a":1,"b":2,"c":3}

second['a'] = "TEST" #{"d":4,"a":"TEST","b":2,"c":3}
second.update(first) #Updating
second #{"d":4,"a":1,"b":2,"c":3} # overrides "a":"TEST"
```

### Iterating over dictionaries
```python
# Iterates over keys
for key in my_dict: # same with for key in my_dict.keys()
	# Iterates over keys
    print(key)  

# Iterates over values
for value in my_dict.values():
    print(value)  

for key, value in my_dict.items():
    # Iterates over key-value pairs
    print(f"{key}: {value}")  

```

### Checking if key/value exists
```python
myself = {
	"name" : "Leejun",
	"dog_person": True,
	"loves_ramen": True 
}

# Testing keys
"name" in myself # True
"name" in myself.keys() # True
"is_student" in myself # False (not an error)

# Testing values
"Leejun" in myself.values() # True
"False" in myself.values() # False
```
### Practice
```python
artist = {
	"first": "Neil",
	"last": "Young",
}
 
# concat first and last name with a space
full_name = artist["first"] + " " + artist["last"]

# format()
full_name = "{} {}".format(artist["first"], artist["last"])

# f string
full_name = f"{artist["first"]} {artist['last']}"
```


getting the value of a dict, but getting 0 if there is none yet
```python
dict1[c] = dict1.get(c, 0) + 1
```