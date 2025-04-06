- [[OOP Intro]]
- name them in singular, CamelCase
- `__init__` method = constructor
	- allows you to define how an object should be set up when it is created
	- can take arguments (parameters) that you want to pass during the object creation process
	- 1st parameter is always `self`, which refers to the instance of the object being created
	- Any time we make a new instance of a class, python automatically looks for the `__init__` method
	- You might not need it if the class only holds methods and no data.....but that's a special case

```python
class User:
	pass # empty class 

# Instantiating a user class = creating a new object
user1 = User()

class User:
	def __init__(self, name, age):
		# print("New user instantiated!)
		self.name = name
		self.age = age

user1 = User("Alice", 19) # python calls init here
print(user1.name) # "Alice"
```
- This is a `User` class with **3 instance attributes** (but no instance methods)
### Instance attributes & methods
```python
class User:
    def __init__(self, first, last, age):
	    # instance attributes
        self.first = first
        self.last = last
        self.age = age

	# instance methods
    def full_name(self):
        return f"{self.first} {self.last}"

    def initials(self):
        return f"{self.first[0]}. {self.last[0]}."

    def fav_food(self, food):
        return f"{self.first}'s favorite food is {food}!"

    def birthday(self):
        self.age += 1
        return f"Happy {self.age}th bday, {self.first}!"
		
user1 = User("Kirby", "Kim", 0)
print(user1.fav_food("seaweed"))
```
- Instance attributes
	- only on the specific instantiated object
- Instance methods
	- operates with a specific instance (object) of the class
	- ALWAYS takes `self` as its first parameter
	- can access and modify instance attributes (i.e., attributes unique to an object of the class
### Class attribute
```python
class User:

	# class attribute (defined only once)
	active_users = 0

	def __init__(self, first, last, age):
		# instance attributes
		self.first = first
		self.last = last
		self.age = age
		User.ative_users += 1 # anytime a new user is instantiated, we add 1

	# instance methods
	def full_name(self):
		return f"{self.first} {self.last}"

# we use the class itself!
print(User.active_users) # 0 
user1 = User("Kirby", "Kim", 0)
print(User.active_users) # 1
```
- Class attributes
	- Attributes **shared by all instances of a class and the class itself**
	- used far less than instance attributes
	- Common use cases
		- keeping track of how many were instantiated
		- default or shared data (car class has 4 wheels)
		- constants (circle class has 3.14 constant)
- Class methods
	- Operates on the class itself, not on instances
	- Often used when you need to perform operations that pertain to the class as a whole, not to specific objects of the class
	- uses the `@classmethod` decorator

### Class methods
```python
class Pet:
	allowed = ['cat', 'dog','fish']
	total_pets = 0
	
	def __init__(self, name, species):
		self.name = name
		if species not in Pet.allowed:
			raise ValueError(f"You can't have a {species} as a pet")
		self.species = species
		total_pets += 1

	@classmethod
	def total_pet_num(cls):
		return f"Total pets: {Pet.total_pets}"
		
print(Pet.total_pet_num()) #Total pets: 0
```

If we have csv files
```python
class User:
	active_users = 0

	def __init__(self, first, last, age):
		self.first = first
		self.last = last
		self.age = age
		User.active_users += 1

	# instance method
	def birthday(self):
		self.age += 1
		return f"Happy {self.age}th bday, {self.first}!"
	
	@classmethod
	def from_string(cls, data_str):
		first,last,age = data_str.split(',')
		return cls(first, last, int(age))
	
user1 = User.from_string("Kirby,kim,0")
print(user1.birthday()) # Happy 1th bday, Kirby!
```

### The `__repr__` method
- `__repr__` method
	- One of sever ways to provide a nicer string representation
```python
user = User1("kirby","kim",0)
print(user) # output: <__main__.User object at 0x7f2a1d62d490>

# adding this
def __repr__(self):
	return f"{self.first} {self.last}"

print(user) #output: kirby kim
```