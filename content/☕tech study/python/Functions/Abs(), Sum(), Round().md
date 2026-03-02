- All works for numbers
### Abs()
```python
abs(-5) # 5
abs(3.444) # 3.444

# if you want float
import math
math.fabs(-4) # 4.0
```
### Sum()
- takes an iterable and an optional start (default is 0)
- sums from that start
- left to right and returns the total
```python
sum([1,2,3]) # 6
sum([1,2,3],10) #starts counting from 10, so 10 + 1 + 2 + 3 = 16
#works for tuples, floats, set, as long as there is numbers
```
### Round()
- returns rounded number rounded to `ndigits` precision after the decimal point
- if `ndigits` is omitted or `None`, it returns the nearest integer to its input
```python
round(10.2) # 10
round(1.21212121,2) # 1.21
```