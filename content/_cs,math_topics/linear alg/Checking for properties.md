### Distributive
```python
# create random vectors
n = 10
a = np.random.randn(n)
b = np.random.randn(n)
c = np.random.randn(n)

# res1 = a.T @ (b + c) = a @ (b + c)
# res2 = (a.T @ b) + (a.T @ c) = (a @ b) + (a @ c)
res1 = np.dot(a, (b + c))
res2 = np.dot(a, b) + np.dot(a, c)
print("Distributive Property:")
print([res1,res2]) # equal
```
### Associative
```python
""" Dot product is NOT ASSOCIATIVE """
# create random vectors
n = 5
a = np.random.randn(n)
b = np.random.randn(n)
c = np.random.randn(n)

res1 = np.dot( a, np.dot(b,c) )
res2 = np.dot( np.dot(a,b), c )
# no longer a dot product, but still a valid operation -> scalar-vector multiplication
# so the result is now a list
print("Associative Property:")
print(res1, res2) # results to reducing to scalar-vector multiplications

## special cases
# 1. One vector is the zeros vector
# 2. a==b==c
```
### Commutative
```python
""" ===== ASSOCIATIVE ===== """
""" Dot product is NOT ASSOCIATIVE """
# create random vectors
n = 5
a = np.random.randn(n)
b = np.random.randn(n)
c = np.random.randn(n)

res1 = np.dot( a, np.dot(b,c) )
res2 = np.dot( np.dot(a,b), c )
# no longer a dot product, but still a valid operation -> scalar-vector multiplication
# so the result is now a list
print("Associative Property:")
print(res1, res2) # results to reducing to scalar-vector multiplications

## special cases
# 1. One vector is the zeros vector
# 2. a==b==c

""" ===== Commutative ===== """
"""
Create a 2 4x6 matrices of random numbers
Use a for loop to compute dot products between corresponding columns
"""
"""
a'b =? b'a

Generate 2 100-element random row vectors, compute dot product a with b, b with a
Generate two 2-element integer row vectors, repeat
"""

a = np.random.randn(100)
b = np.random.randn(100)

res1 = np.dot(A, B)
res2 = np.dot(B, A)
print([res1, res2])

c = np.random.randint(0,10,size=(2,)) # we can actually use lists too
d = np.random.randint(0,10,size=(2,))
# print(f"c:{c}")
# print(f"d:{d}")
resa = np.dot(c,d)
resb = np.dot(d,c)
print([resa, resb]) # equal
# its commutative!
```
- Distributive - YES
- Associative - NO
- Commutative - YES
	- Scalar multiplication is commutative, so the dot product, which is made out of scalar multiplication, is also commutative