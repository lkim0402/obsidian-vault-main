- https://leetcode.com/problems/number-of-adjacent-elements-with-the-same-color/description/

#### good solution
- we can just check when we're updating the index
```python
def colorTheArray(self, n: int, queries: List[List[int]]) -> List[int]:
	ans = []
	colors = [0] * n

	totalPairs = 0
	for query in queries:
		i = query[0]
		color = query[1]

		# subtracting adj pairs for the current value, since we will update the value
		pair = 0
		if colors[i] != 0:
			if i > 0 and colors[i] == colors[i - 1]: # checking left
				pair -= 1
			if i < len(colors) - 1 and colors[i] == colors[i + 1]: # checking right
				pair -= 1
			
		# updating color
		colors[i] = color
		
		# freshly counting the num of adj pairs w/ new value
		if colors[i] != 0:
			if i > 0 and colors[i] == colors[i - 1]: # checking left
				pair += 1
			if i < len(colors) - 1 and colors[i] == colors[i + 1]:# checking right
				pair += 1

		totalPairs += pair
		ans.append(totalPairs)

	return ans
```
- The trick
	- just update the values when you're updating it, and have a separate total aggregate pair
	- check for pairs (L and R) before you update the value - we're removing the pairs that are associated with the current value
	- after updating the value, check for pairs (L and R), and add it to the total aggregate pair
- track total state over time.. a delta update

#### Inefficient solution
```python
def colorTheArray(self, n: int, queries: List[List[int]]) -> List[int]:
	ans = []
	colors = [0] * n
	
	"""
	[1,1,1,1,1]
	"""

	for query in queries:
		index = query[0]
		color = query[1]
		colors[index] = color

		# getting the adjacent
		pair = 0
		for i in range(len(colors) - 1):
			if colors[i] == colors[i+1] and colors[i] != 0:
				print(f'colors[{i}]: ', colors[i])
				print(f'colors[{i+1}]: ', colors[i+1])  
				pair += 1
		
		ans.append(pair)
	
	print(colors)
	print(ans)

	return ans
```