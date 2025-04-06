
```python
class Human:
	def __init__(self, first, last, age):
		self.first = first
		self.last = last
		self._age = max(0,age) #prevent negative age
		
	@property
	def age(self):
		return self._age # acts as getter

	@age.setter
	def age(self, age):
			self._age = max(0,age)
	#  use properties, not explicit getter/setter methods
	# def get_age()

jane = Human("Jane", "Goodall", 50)
print(jane.age) # 50 -> age acts like an attribute!
	
```
- `@property`
	- The property age function acts as a getter
	- Allows you to access methods like attributes, which makes the code cleaner and more intuitive
	- you can encapsulate the internal representation of the data
		- prevents direct access to the underlying data structure, promoting better encapsulation
- If you try to `jane.age = 10`
	- Error message: `property 'age' of 'Human' object has no setter`
- No need to define additional explicit setter and getter methods in the class
- Example: Employee
```python

"""======== w/o email property ========"""
class Employee:
	def __init__(self, first, last):
		self.first = first
		self.last = last
		self.email = f"{first}.{last}@gmail.com"
	
	def fullname(self): 
		return f"{self.first} {self.last}"

emp1 = Employee('John','Smith')
print(emp1.email) #John.Smith@gmail.com
emp1.first = 'Jim'
print(emp1.email) # Still John.Smith@gmail.com!

"""======== w/ email + fullname property ========"""
class Employee:
	def __init__(self, first, last):
		self.first = first
		self.last = last
	
	@property
	def email(self):
		return f"{self.first}.{self.last}@gmail.com"
	
	@email.setter
	# setting first and last names according to new email
	def email(self, new_email):
		split = new_email.split("@")
		if len(split) != 2:
			raise ValueError("Not valid email format!")
		name = split[0] # split equals ['John Smith', 'gmail.com]

		split2 = name.split(".")
		if len(split2) != 2:
			raise ValueError("Not valid name format!")
		
		first, last = split2
		self.first = first
		self.last = last
	
	@property
	def fullname(self): 
		return f"{self.first} {self.last}"
	
	@fullname.setter
	def fullname(self, new_name):
		first, last = new_name.split(" ")
		self.first = first
		self.last = last

emp1 = Employee('John','Smith')
print(emp1.email) #John.Smith@gmail.com
emp1.first = 'Jim'
print(emp1.email) # John.Smith@gmail.com!

emp2 = Employee('Kate', 'Smith')

emp2.fullname = 'Kate Green'
print(emp2.email)
emp2.email = "hello.kim@gmail.com"
print(emp2.email)

```
- It basically allows you to set properties like email and fullname while updating all attributes