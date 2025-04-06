> One of the most powerful features of python
> - shorthand syntax to return new lists
- Related: [[List]]
- you don't use list comprehension if your return value isn't a list

```python
[item for items in iterable]

nums = [1,2,3]
new_nums = [x*10 for x in nums] # [10,20,30]

name = "Lucy"
[char.upper() for char in name] #["L", "U", "C", "Y"]

# we can iterate over ranges too
[num*10 for num in range(1,6)] # [10,20,30,40,5]

# returning if it's truthy or falsey
[bool(val) for val in [0, [], '']] #[False, False, False]
```
- python internally creates a loop to process each element
- great for converting elements into a new list
	`string_list = [str(num) for num in num_list]`

#### With conditional logic
```python
numbers = list(range(1,7))

# if keyword (at the back)
evens = [num for nums in numbers if num % 2 == 0]
odds = [num for nums in numbers if num % 2 == 1]

# if-else keyword (to the front)
[num*2 if num % 2 == 0 else num/2 for num in numbers]
# [0.5, 4, 1.5, 8, 2.5, 12]

with_vowels = "This is so much fun!"
''.join(char for char in with_vowels if char not in "aeiou")
#"Ths s s mch fn!"

```
- The [] is a wrapper for returning lists

#### Nested lists
- Lists in other lists => Multidimensional lists
- Usage
	- Complex data structures and matrices ([[Machine Learning]], linear algebra)
		- [[NumPy]]
	- Game board/mazes
	- Rows and Columns for visualizations, tabulation and grouping data
	- Anything data science in python
```python
nested_list = [[1,2,3], [4,5,6], [7,8,0]]

nested_list[-1] # [7,8,0]
nested_list[0][1] #2
nested_list[2][2] #0
nested_list[1][-1] #6

""" Nested list comprehension """
nested_list = [[1,2,3], [4,5,6], [7,8,0]]
[(print(val) for val in list) for list in nested_list]

"""Examples"""
# ===== Representing a matrix ===== 
#[[1,2,3], [1,2,3], [1,2,3]]
matrix = [[num for num in range(1,4)] for num in range(1,4)]
matrix = [[in_num for in_num in range(1,4)] for out_num in range(1,4)]
# Can use different variables for the nested loops

# ===== Representing a game board ===== 
board = [['X' if num % 2 == 1 else 'O' for num in range(1,4)] for num in range(1,4)]
# [['X', 'O', 'X'], ['X', 'O', 'X'], ['X', 'O', 'X']]
```