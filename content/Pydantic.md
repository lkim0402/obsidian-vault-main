
>[!Pydantic]
>A data validation library for [[python]]
>- Makes it super easy to define **schemas** & export it to JSON

- Works with [[Type annotations|python type annotations]]
	- but rather than static type checking, they are actively used at runtime for data validation and conversion
- provides built-in methods to serialize/deserialize models to/from JSON, dictionaries, etc
	- [[JSON]], [[Python Dictionary (Hashmap)]]
	- [[Request and response model| serialization/deserialization reminds me of the request and response model (backend eng)]]
- [[LangChain]] leverages Pydantic to create JSON Scheme describing function
	- [[📌OpenAI Function Calling with LCEL (+pydantic)]]

# Syntax

- It's similar to making a class. normal python:
	- [[Classes in Python]] (python)
```python
class User:
	def __init__(self, name:str, age: int, email: str):
		self.name = name
		self.age = age
		self.email = email
```

- in pydantic, we just list the attributes and types
```python
rom pydantic import BaseModel, Field

class pUser(BaseModel):
    name: str
    age: int
    email: str
foo = User(name="Joe", age=32, email="joe@email.com")
print(foo.name) # 'Joe'
```

- this will fail due to type validation (`age` is invalid)
```python
foo_p = pUser(name="Jane", age="bar", email="jane@gmail.com")
```
```
ValidationError: 1 validation error for pUser
age
  value is not a valid integer (type=type_error.integer)
```

- we can make a class
	- we can nest these data structures!
```python 
class Class(BaseModel):
	students: List[pUser]

obj = Class(
    students=[pUser(name="Jane", age=32, email="jane@gmail.com")]
) 
```

# `Field`
- used for **customizing model fields**
	- setting default values
	- adding validation and constraints
		- `max_length`, `gt`,`lt` etc
	- providing metadata
		- title, description, examples
	- serialization/deserialization
		- u can use arguments like `alias` to map an incoming data field name to a different name in the Pydantic model, or `repr` to control whether the field is included in the model's string
```python 
from uuid import uuid4
from pydantic import BaseModel, Field

class User(BaseModel):
    # Field with a default value and description
    name: str = Field(default="John Doe", description="The name of the user")

    # Field with a dynamic default using default_factory
    # Note: default and default_factory are mutually exclusive
    id: str = Field(default_factory=lambda: uuid4().hex)

    # Field with validation constraints (must be between 0 and 120)
    age: int = Field(gt=0, lt=120)

user = User(age=42)
print(user)

```