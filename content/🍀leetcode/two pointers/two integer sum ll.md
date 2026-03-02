- https://neetcode.io/problems/two-integer-sum-ii
```python
def twoSum(self, numbers: List[int], target: int) -> List[int]:
	"""
	[1,2,3,4]
	 ^     ^
	 -> if sum < target, move l
	 -> if sum > target, move r
	"""

	l = 0
	r = len(numbers) - 1

	while (l < r):
		sum = numbers[l] + numbers[r]
		if sum == target:
			return [l + 1,r + 1] # because it's 1-indexed lmao
		if sum < target:
			l += 1
		elif sum > target:
			r -= 1             
```
- we want to return the indices - 1-indexed
	- read the question properly because i missed this and had to stare at the problem again