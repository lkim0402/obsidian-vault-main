- When you have all/a subset 4 of these, this is the ordering
	1. parameters
	2. [[*args parameter]]
	3. default parameters
	4. [[**kwargs parameter]]

```python
def display_info(a,b,*args,first_name="Leejun",**kwargs):
	return [a,b,args,first_name,kwargs]

print(display_info(1,2,3,last_name="Kim", job="Student"))
"""
a = 1
b = 2
args = (3,) because it only captures positional args
first_name = "Leejun"
kwargs = {'last_name':'Kim', 'job':'Student'}
"""
# returns [1,2,(3,),"Leejun",{'last_name':'Kim', 'job':'Student'}]
```