- A key feature of OOP is the ability to define a class which inherits from another class (a "base" or "parent" class)
- In Python, inheritance works by passing the parent class as an argument to the definition of a child class
```python
class Animal:
	def make_sound(self,soud):
		print(sound)
	cool = True

class Cat(Animal):
	pass

gandalf = Cat()
gandalf.make_sound("meow") # "meow"
gandalf.cool # True
```

Using `super()`
```python
class Animal:
	def __init__(self, name, species):
		self.name = name
		self.species = species
    
	def __repr__(self):
		return f"{self.name} is a {self.species}"
	
class Cat(Animal):
	def __init__(self, name, breed, toy):
		super().__init__(name, species = "Cat")
		# Animal.__init__(self, name, species = "Cat")
		# no need to set these: (duplicates with Animal class)
			# self.name = name
			# self.species = species
		self.breed = breed
		self.toy = toy 


cat1 = Cat("kitty", "Scottish Fold", "idk the breed","string")
print(cat1.breed)
```

### Multiple inheritance
```python
class A:
    def __init__(self,name):
        print("A init")
        self.name = name
    
    def greet(self):
        return f"Hello!"

class B:
    def __init__(self,name):
        print("B init")
        self.name = name
    
    def greet(self):
        return f"Hellooooo!"
	
	def cat(self):
		return "meow"

class C(A,B):
    def __init__(self,name):
        print("C init")
        super().__init__(name=name)
        # if we want to explicitly call B's init
        # B.__init__(self,name=name) 

test = C("test!")
print(test.cat()) # "meow" -> meow from B
print(test.greet()) # "Hello!" -> greet from A

# With debug statements
# C init
# A init 
# Hello!
```
- Just generally don't do mutiple inheritances
- So the order of inheritance matters -> if there's overlapping methods, it takes the 1st class
	- Method Resolution Order(MRO)

### Method Resolution Order(MRO)
- The order in which Python will look for methods on instances of that class 
	- Python sets an MRO whenever you create a class
- How to programmatically reference the MRO
	- `__mro__` attribute on the class
	- use the `mro()` method on the class
	- use the builtin `help(cls)` method (more readable human format)
```python
class Mother:
    def __init__(self):
        #self.eye_color = "brown"
        self.hair_color = "dark brown"
        self.hair_type = "curly"
    
    
class Father:
    def __init__(self):
        self.eye_color = "blue"
        self.hair_color = "blond"
        self.hair_type = "straight"
    
    
class Child(Mother, Father):
    pass
    
    
j = Child()
    
print(j.eye_color) #ERROR
```
- `j.eye_color` gives ERROR
	- When the Child class inherits from both Mother and Father, it calls the `__init__` method of only one of those classes (in this case, Mother),
	- Solution is to manually call the `__init__` methods of both Mother and Father
```python
class Child(Mother, Father):
    def __init__(self):
        Mother.__init__(self)  # Call Mother's __init__
        Father.__init__(self)  # Call Father's __init__
```