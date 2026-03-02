- [[NumPy]]
- Mentioned in  [[Shapes & dimensions]] 

### Getting access
```python
arr = np.array(
   [[1,2,3,6],
     [4,5,6,2],
     [7,8,9,4]]
)

arr[0] # [1,2,3,6]
arr[0][0] # 1
arr[0][1] # 2
arr[0,1] # 2 (same notation!)

""" Getting cols"""
arr[:,0] # arr[1,4,7]
```
- `:` means select everything in that dimension, in our case, select everything in that row

### Transpose
```python
arr.T
# [[1 4 7]
#  [2 5 8]
#  [3 6 9]
#  [6 2 4]]
```

### np.exp()
- computes the **exponential** of all elements in the input array
- calculates $e^x$ for each element xxx in $x$ the array
```python
np.exp(arr)
#[[2.71828183e+00 7.38905610e+00 2.00855369e+01 4.03428793e+02]
# [5.45981500e+01 1.48413159e+02 4.03428793e+02 #7.38905610e+00]
# [1.09663316e+03 2.98095799e+03 8.10308393e+03 5.45981500e+01]]
np.exp(random_list)
```
- So many functions allows you to put a list (not a np array) and it will still worka dn return a np array
### Matrix multiplication
```python
A = np.array([[1,2],[3,4]])
B = np.array([[1,2,3],[4,5,6]])

A.dot(B)
# [[9,12,15],
#  [19,26,33]]
```