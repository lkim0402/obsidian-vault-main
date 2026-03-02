> A specific type of iterator that is created using a generator function (has the `yield` keyword). It automatically implements the `__iter__()` and `__next__()` methods for you.
	- quick and easy way of creating iterators (no need to manually implement the `__iter__()` and `__next__()`)
- [[Iterator vs Iterable]]
- Are a subset of iterators: Every generator is an iterable, but not every iterator is a generator
- they do not generate all things and store them in the memory but they generate the value on the fly, one at a time
- can be created with ==generator functions==
	- generator functions use the `yield` keyword
- can be created with ==generator expressions==
	- like  [[List Comprehension]] but using `()` and not `[]`
	- ex. `squares_gen = (x**2 for x in range(10))`

# Generator Functions

| Functions                              | Generator Functions               |
| -------------------------------------- | --------------------------------- |
| uses `return`                          | uses `yield`                      |
| returns once                           | can yield multiple times          |
| when invoked, returns the return value | when invoked, returns a generator |

Our first generator function that returns a generator
```python
def count_up_to(max):
	count = 1
	while count <= max:
		yield count 
		count += 1

count_up_to_(5)
# <generator object count_up_to at 0x102c8ea0>
```
- `yield`
	- pauses the function and saves its state, the function can be resumed later from where it was paused
	- returns the value to the caller
- When you call `count_up_to()`, it doesn't run the function immediately, but it **returns a generator**. This generator produces values one at a time when you call `next()` on it or loop over it (e.g., in a for loop).
```python
gen = count_up_to(5)

# Call next() to get values from the generator one by one
print(next(gen))  # Output: 1
print(next(gen))  # Output: 2
print(next(gen))  # Output: 3
```
- The first `next(gen)` call runs the function until it hits the `yield count`, returning `1`, then pauses.
- The second `next(gen)` call resumes from where it paused, increments `count`, yields `2`, and pauses again.
```python
for num in count_up_to(3):
    print(num)
```
- In for loops, Python automatically calls `next()` on the generator for each iteration of the loop
- Steps
	- Step 1: At the start of the loop, iter(count_up_to(5)) is called. Since count_up_to() is a generator function, it returns a generator object, which acts as the iterator.
	- Step 2: Python calls next() on that generator to get the first value. The generator yields 1.
	- Step 3: The value 1 is printed.
	- Step 4: The next iteration of the loop calls next() again, which resumes the generator and yields 2, and so on.
	- Step 5: This continues until the generator raises StopIteration after yielding 5, and the loop stops.
```python
counter = count_up_to(10)
list(counter)
# [1,2,3,4,5,6,7,8,9,10]
# just goes through everything and adds to list
```

### Short exercises
```python
"""
Week generator
"""
def week():
    week = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
    # current = 0
    # while current <= len(week) - 1:
    #     yield week[current]
    #     current += 1
    for day in week:
	    yield week
		# much better than while loop...wtf why didn't I do this?

"""
Write a function called **yes_or_no**, which returns a generator that first yields `yes` , then `no` , then `yes` , then `no` , and so on.
"""

def yes_or_no():
    while True:
        yield 'yes'
        yield 'no'
```

### Infinite generator
> Beat box application lmao
```python
""" Ver 1"""
def current_beat():
    beat = [1,2,3,4]
    cur = 0
    while True:
        if cur == 4: cur = 0
        yield beat[cur]
        cur += 1

print(current_beat())
for beat in current_beat():
    print(beat)

""" Ver 2 - same thing but shorter """
def current_beat():
    while True:
        yield 1
        yield 2
        yield 3
        yield 4

""" Ver 3 - using % (omg....진짜 개천재같음..갈길이 멀다..) """
def current_beat()"
	i = 0
	while True:
		yield i % 4 + 1
		i += 1
```

# Generator Expressions
- like  [[List Comprehension]] but using `()` and not `[]`
	- ex. `squares_gen = (x**2 for x in range(10))`

```python
def nums():
	for num in range(1,10):
		yield num

g = nums()
# <generator object nums at 0x000001>
next(g) # 1
next(g) # 2

# Same thing
g = (num for num in range(1,10))
# <generator object <genexpr> at 0x0000100>
next(g) # 1
next(g) # 2

# operations still work
sum([num for num in range(1,10)]) # 45 (list comprehension)
sum(num for num in range(1,10)) # 45 (generator expression)
```