# `json.dumps()` and `json.dump()`
- They're both for serializing Python objects into JSON format, but they differ in their output destination
- 📌`indent`
	- Use this to indicate each level of indentation in the JSON output (number of spaces)
## `json.dumps()`
- Converts a Python object (like a dictionary or list) into a JSON-formatted string.
	- Returns a **string** containing the JSON representation of the Python object. 
	- The "s" in `dumps` stands for "dump string."
```python
import json
a = ['foo', {'bar': ('baz', None, 1.0, 2)}]
test = json.dumps(a)
print(test)
#["foo", {"bar": ["baz", null, 1.0, 2]}]
```
- `dumps
	- Converts Python data into json
	- ```None`` -> null
	- `''` -> `""`
	- tuples -> list

If we have a custom class:
```python
import json

class Cat:
	def __init__(self, name, breed):
		self.name = name
		self.breed = breed
c = Cat("Cheese", "Tabby")

j = json.dumps(c.__dict__)
print(j) # {"name": "Cheese", "breed": "Tabby"}
```
## `json.dump()`
- Serializes a Python object into a JSON-formatted string and writes it directly to a file-like object (e.g., a file opened in write mode).
	- Writes the JSON data directly to the specified file, rather than returning a string.
	- Ideal when you want to save Python data structures directly into a JSON file.
```python
import json

data = {
    "name": "Bob",
    "age": 25,
    "city": "London"
}

with open("output.json", "w") as f:
    json.dump(data, f, indent=4)
```
# `jsonpickle`

```sh
python3 -m pip install jsonpickle
```

```python
import jsonpickle

class Cat:
	def __init__(self, name, breed):
		self.name = name
		self.breed = breed
c = Cat("Cheese", "Tabby")

frozen = jsonpickle.encode(c)
print(frozen)
# {"py/object": "__main__.Cat", "name": "Cheese", "breed": "Tabby"}

unfrozen = jsonpickle.decode(frozen)
print(unfrozen)
# <__main__.Cat object at 0x7fc1d310b2c0>
```