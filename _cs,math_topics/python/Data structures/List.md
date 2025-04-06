> Just a collection or grouping of items!
> `ints`, `floats`, `bools`, and even other lists or [[Dictionary (Hashmap)|dictionaries]]!
- Advanced: [[List Comprehension]]
- an empty list is inherently falsey
- It's a **data structure**, where we can *store other data types inside of it*.
#### Built in functions for lists
- functions are defined at a global level
- called with parenthesis
```python
"""=== These are NOT specific to lists ==="""
#length
my_list = ['a', 1, True]
len(my_list) # 3

#list() fuction (converting range to list)
r = range(1,5)
print(r) # range(1,5)
list(r) # [1,2,3,4]
list(range(1,5)) #same thing
```
- [[Looping in Python#`range`|range]]
#### Methods specific to lists
- Methods are bound to objects or class
- almost always ends with `()`
```python
"""
You can use then by adding a . at the end of list
"""

my_list = [3,2,5]

""" adding """
my_list.append(4)
print(my_list)  # Output: [3,2,5,4]

""" adding more than 1 value """
my_list.extend([9,10])
print(my_list)  # Output: [3,2,5,4,9,10]

""" adding anywhere you like """
my_list.insert(1, "hi")
print(my_list) # Output: [3,"hi",2,5,4,9,10]
my_list.insert(-1, "The end!")
print(my_list) # Output: [3,"hi",2,5,4,9,"The end!",10]
# inserts at the previous list (before it's modified)

""" emptying list """
my_list.clear() # my_list = []

""" removing element """
# put in index and return the item you want to remove
# if no index, it removes & returns last item
my_list = [1,2,3,4]
my_list.pop() #4
my_list.pop(1) #2

my_list.remove(1) # my_list = [2,3,4], removes item
# Throws a ValueError if item not found
```

Less common methods
```python
""" returns the index of the specified item in list"""
my_list = [5,5,6,7,8,5,8]
my_list.index(5) #0
my_list.index(7) #2

# specifying start and end index
my_list.index(5,1) #1 (searches from index 1)
my_list.index(5,2,-1) #5

""" returns # of times the item occurs in list """
my_list.count(5) #3

""" reverses list (in place)"""
num = [1,2,3,4]
num.reverse() #num is now [4,3,2,1]

""" sorting """
num = [4,2,3,1]
my_list.sort() # [1,2,3,4] (in place)

""" converting list to string """
# not really a method of list
# returns a new string
words = ['Hello', 'World']
' '.join(words) #'Hello World'
#(or)
'/'.join(words) #'Hello/World
```
#### Accessing values
- Like ranges, lists ALWAYS start counting at 0
```python
my_list = ['a', 1, True]

#accessing elements w/ index
print(my_list[0]) # 'a'
print(my_list[3]) #index out of bounds error

#negative index starts counting backwards
print(my_list[-1]) # True
print(my_list[-2]) # 1

#checking if value exists
print('a' in my_list) # True
print(3 in my_list) # False
```
#### Iterating over lists
```python
numbers = [1,2,3,4]

for num in numbers:
	print(num)

i = 0
while i  < len(num):
	print(numbers[i])
	i += 1
```

#### Slicing
- returns copies of portions of lists
```python
"""
some_list[start:end:step]
"""

some_list = [5,2,6,1,8]
"""start"""
some_list[1:] # [2,6,1,8]
some_list[4:] # [8]
# neg num -> slice # times from end
some_list[-1:] # [8] getting last elem
some_list[-3:] # [6,1,8]

"""end (exclusive)"""
some_list[:1] # [5]
some_list[:4] # [5,2,6,1]
some_list[:99] # [5,2,6,1,8] (all)
some_list[:-1] # [5,2,6,1]

#printing everything
some_list[:]
some_list[0:]

#start and neg end (exclusive)
some_list[1:-1] # [2,6,1]
some_list[2:-1] # [6,1]

"""step"""
some_list = [1,2,3,4,5,6,7,8]  
some_list[::-1]  # [8, 7, 6, 5, 4, 3, 2, 1]
some_list[1:6:-2] # INVALID!
some_list[6:1:-2] # [7,5,3]
some_list[:1:-1] # [8,7,6,5,4,3] Starts from the end
some_list[1:3] = "b" #[1,"b",4,5,6,7,8]   
some_list[::-1] # full reversal: [8,7,6,5,4,3,2,1]
```

NOTE
```python
some_list= [1,2,3,4,5,6,7,8]
some_list[::2] = "hi"
#Traceback (most recent call last):
# File "<stdin>", line 1, in <module>
# ValueError: attempt to assign sequence of size 2 to extended slice of size 4

some_list[1:4:2] = "hi"
[1, 'h', 3, 'i', 5, 6, 7, 8]
 0   1   2   3
 # from index 1 to 3 and skipping by 2, replace with "hi"
 # string is an iterble so pythonsplits it

some_list= [1,2,3,4,5,6,7,8]  
some_list[1:4] = "hello"    
[1, 2, 3, 4, 5, 6, 7, 8] # original
[1,'h','e','l','l','o',5,6,7,8] # changed
# 2, 3, 4 are replaced by "hel", and "lo" is added

color_list = ["blue", "white"]
color_list[0][::-1] # 'eulb'

"hellooooooooo"[0] # h
"hellooooooooo"[::-1] # 'ooooooooolleh'

```

SWAPPING VALUES
```python

names = ["kirby", "yoshi"]
names[0], names[1] = names[1], names[0]
# [ "yoshi", "kirby"]
```
- shuffling
- sorting 
- a lot of algorithms in general