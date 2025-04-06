- [[NumPy]]
- Related: [[List|Lists in Python]]
- https://colab.research.google.com/drive/1VdLyfnop7Tt4wx4BmS6WHSSQg3n5wXrX?pli=1&authuser=1

- If you apply a function to a numpy array, it very often operates element wise.
- A numpy array exists specifically to do math!
	- Also they are more memory efficient, faster, and more convenient!

```python
import numpy as np

L = [1,2,3]
A = np.array([1,2,3])

# the for loops does the same thing for both
for i in L:
	print(i)

for i in A:
	print(i)

L.append(4) # L = [1,2,3,4]
A.append(4) # ERROR! (arrays can't be changed)

"""
L = [1,2,3,4]
A = [1,2,3]
"""

L + [5] # L = [1,2,3,4,5]
A + [5] # A = [6,7,8] (BROADCASTING)
A + np.array([5]) # A = [6,7,8]

L * 2 # L = [1,2,3,4,1,2,3,4], same as L + L
A * 2 # A = [2,4,6], same as A + A

# examples of functions
np.sqrt(A)
np.log(A)
```
### Broadcasting
- A numpy array **broadcasts**!
	- NumPy performs element-wise operations on arrays of different shapes
	- The 2nd item gets "broadcasted" to the original item

```python
"""Case 1: Scalar and array"""
A = np.array([1,2,3])
# Broadcasting scalar 1 to each element
A + 1 # [2,3,4]

"""Case 2: Arrays with different shapes"""
# shape (2,3)
A = np.array( 
	[
	  [1,2,3],
	  [4,5,6]
	]
)
# shape (3,)
B = np.array(
	[10,20,30]
)
# Broadcasting B to match the shape of A
result = A + B  
# [[11 22 33]
#  [14 25 36]]

"""Case 3: Mismatched Shapes that Work with Broadcasting"""
```
- Related: [[Shapes & dimensions]] 


