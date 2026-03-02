- Iterable
	- An object that has an `__iter__` method which returns an iterator, or which defines a `__getitem__` method that can take indexes
		- lists, strings, tuples, files, etc
	- So an **iterable** is an object that you can get an **iterator** from
- Iterator
	- an object with state that remembers where it is during iteration
	- object that defines how to actually do the iteration--specifically what is the next element
	- has a `__next__` method that
		- returns the next value in the iteration
		- updates the state to point at the next value
		- keeps returning next item until it raises a `StopIteration` error
	- an object used to iterate over an iterable 
- Example
	- `"HELLO"` is an iterable, not an iterator
	- `iter("HELLO")` returns an iterator

### Example 
- link: https://stackoverflow.com/questions/9884132/what-are-iterator-iterable-and-iteration
So what does Python interpreter think when it sees `for x in obj:` statement?
> Look, a `for` loop. Looks like a job for an iterator... Let's get one. ... There's this `obj` guy, so let's ask him.
> 
> "Mr. `obj`, do you have your iterator?" (... calls `iter(obj)`, which calls `obj.__iter__()`, which happily hands out a shiny new iterator `_i`.)
> 
> OK, that was easy... Let's start iterating then. (`x = _i.next()` ... `x = _i.next()`...)

- Basically, when you have a for loop like `for c in "helloworld"`, the string `"helloworld` is an iterable, and Python automatically gets an iterator from it by calling the `__iter__()` method
	- on each iteration of the loop, Python calls the `__next__()` method of the iterator, which keeps track of the character in the string

### Custom for loop
```python

""" 1st ver """

def my_for(iterable):
    it = iter(iterable) # getting the iterator
    # for i in range(len(iterable)):
    #     print(next(it))
    while True:
        try:
            print(next(it))
        except StopIteration:
            print("end of iterator!")
            break

my_for("hello")
# h
# e 
# l
# l
# o
# end of iterator!

""" 2nd ver - with func """
# we add a function that we'd like to do
def my_for(iterable, func):
	it = iter(iterable) # getting the iterator
	while True:
		try:
			i = next(it)
		except StopIteration:
			break
		else:
			func(i)
my_for([1,2,3], print)
# 1
# 2
# 3

def square(x):
	print(x*x)

my_for([1,2,3], square)
# 1
# 4 
# 9
```

### Custom iterable/iterator
```python

""" Making a custom Iterable Class - using iter"""

class Counter:
    def __init__(self, low, high):
        self.low = low
        self.high = high

    # iter(Counter) will return a iterator, which will have a 'next'
    def __iter__(self):
        return iter(range(self.low, self.high))
    
for n in Counter(50,55):
    print(n)

# lkim@fedora:~/Desktop/python_practice$ python iterators.py 
# 50
# 51
# 52
# 53
# 54

""" Making a custom Iterable Class - using custom class"""

class Counter:
    def __init__(self, low, high):
        self.low = low
        self.high = high

    # iter(Counter) will return a iterator, which will have a 'next'
    def __iter__(self):
        return CountIterator(self.low, self.high)

class CountIterator:
	def __init__(self, low, high):
		self.current = low
		self.high = high
		
	def __next__(self):
		if self.current < self.high:
			cur = self.current
			self.current += 1
			return cur
		raise StopIteration
    
for n in Counter(50,55):
    print(n)

# lkim@fedora:~/Desktop/python_practice$ python iterators.py 
# 50
# 51
# 52
# 53
# 54

""" Making a custom Iterable/Iterator Class - Counter"""
# we can have BOTH by letting __iter__ return self

class Counter:
	# both classes have the same init method, so ig we can merge them
    def __init__(self, low, high):
        self.current = low
        self.high = high

	# the iter function returns the object, which refers to the same instance in memory, which ALSO has __next__ defined
    def __iter__(self):
        return self 
        
	def __next__(self):
		if self.current < self.high:
			cur = self.current
			self.current += 1
			return cur
		raise StopIteration
    
for n in Counter(50,55):
    print(n)

# lkim@fedora:~/Desktop/python_practice$ python iterators.py 
# 50
# 51
# 52
# 53
# 54
```