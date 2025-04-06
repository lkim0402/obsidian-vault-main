### `json`
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

### `jsonpickle`

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