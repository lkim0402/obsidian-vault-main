```python
def numRabbits(self, answers: List[int]) -> int:
	# [1,1,2]
	for i, a in enumerate(answers):
		d[a] = d.get(a, 0) + 1
	print(d)

	"""
	{ 1:2, 2:1 }
	
	"""

	total = 0
	for ans,rabbitNum in d.items():
		group_size = ans + 1
		num_groups = math.ceil(rabbitNum / group_size)
		total += num_groups * group_size
	
	return total
```

#### the trick
- ex: `[2,2,2,2]`
	- `{2:4}`
	- each rabbit is saying "i am part of a group of 1 (me) + 2 = 3"
	- but there are 4 rabbits saying that, and each group can only hold 3  max
	- so there has to be at least 4 rabbits / 3 rabbits per group = 2 groups of 3 rabbits  
		- ceil(4/3) = 2 groups
		- ceil and not floor because floor would undercount
	- 2 groups of 3 rabbits = 6 rabbits in total