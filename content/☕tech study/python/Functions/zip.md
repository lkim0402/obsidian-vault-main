- makes an iterator that aggregates elements from each of the iterables
- Returns an iterator of tuples, where the `i`-th tuple contains the `i`-th element from each of the argument sequences or iterables
- length of the arguments doesn't have to be the same!!
	- `zip()` will stop creating tuples as soon as the shortest iterable is exhausted
- Related: [[Tuples]]

```python
first_zip = zip([1,2,3],[4,5,6]) # order matters
list(first_zip) # [(1,4),(2,5),(3,6)]
dict(first_zip) # {1:4, 2:5, 3:6}
```

### Using the * argument
- [[Using * as an argument (iterable unpacking)]]
```python
five_by_two = [(0,1),(1,2),(2,3),(3,4),(4,5)]
list(zip(*five_by_two))
#[(0,1,2,3,4),(1,2,3,4,5)]
```


### Some exercises
```python
def interleave(a,b):
    #"cat" "dog"
    zipped = zip(a,b) #[("c","d"),("a","o"),("t","g")]
    joined_pair = ("".join(pair) for pair in zipped) #("cd","ao","tg")
    joined = "".join(joined_pair) #"cdaotg"
    return joined

"""
Write a function called triple_and_filter. This function should accept a list of numbers, filter out every number that is not divisible by 4, and return a new list where every remaining number is tripled.
"""
# my solution
def triple_and_filter(nums):
    a = list(filter(lambda x:x % 4 == 0, nums))
    return [num*3 for num in a]
    
# official solution
def triple_and_filter(lst):
     return list(filter(lambda x: x % 4 == 0, map(lambda x: x*3, lst)))

```