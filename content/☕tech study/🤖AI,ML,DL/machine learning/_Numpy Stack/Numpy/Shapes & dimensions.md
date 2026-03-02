- [[NumPy]]
- **Shape** = the number of elements in each dimension
	- NumPy arrays have an attribute called `shape` that returns a [[Tuples|tuple]] with each index having the number of corresponding elements
	- Generally:
		- shape = ( ...., `# of 3D arrays`, `# of 2D arrays`, rows, columns)
- Dimension = the number of axes it has
	- The number of levels of nesting the array has (the number of square brackets!!)
	- Example
		- an np array with shape `(5,6,3)` ->  dimension of 3
		- `np.array([[1,2,3],[1,2,3]])` -> dimension of 2
- This page explains the shapes and dimensions
	- More covered in [[Matrices]]

> Pro tip! Count the number of square brackets `[`
## 1D array
```python
import numpy as np

arr = np.array([1,2,3,4])
# one dimensional array of 4 elements
print(arr.shape) # (4,)
```
- concept of rows and columns apply to NumPy arrays only if the dimension is more than 1
	- For a 1D array like this, there are no rows and cols, so `.shape` just returns the # of elements

## 2D array
```python
arr = np.array(
   [[1,2,3,6],
     [4,5,6,2],
     [7,8,9,4]]
)
print(arr.shape) # (3,4)
print(arr)
# [[1 2 3 6]  
#  [4 5 6 2]  
#  [7 8 9 4]]
```
- `arr` has 3 rows and 4 columns
- There are 2 square brackets `[[`-> this indicates that this is a 2D array
- A 2D array is just a collection of 1D arrays.
	- A 3D array is a collection of 2D arrays and goes on
- So `arr` is a 2D array made out of 3 1D arrays

## 3D array
```python
arr = np.array([[[1,2,3],[4,5,6],[7,8,9],[11,12,13]],[[10,20,30],[40,50,60],[70,80,90],[11,12,13]]])
print(arr.shape) # (2,4,3)
print(arr)
#[[[ 1  2  3]  
#  [ 4  5  6]  
#  [ 7  8  9]  
#  [11 12 13]] 
# [[10 20 30]  
#  [40 50 60]  
#  [70 80 90]  
#  [11 12 13]]]
```
- `arr.shape` is `(2,4,3)`
	- `arr` now has 2 **2D arrays.**
	- Each of those 2D arrays is made up of 4 **1D arrays** (rows).
	- Each of those 1D arrays contains 3 elements.