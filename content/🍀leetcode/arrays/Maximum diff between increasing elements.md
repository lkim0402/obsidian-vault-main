- Given a **0-indexed** integer array `nums` of size `n`, find the **maximum difference** between `nums[i]` and `nums[j]` (i.e., `nums[j] - nums[i]`), such that `0 <= i < j < n` and `nums[i] < nums[j]`.
- Return _the **maximum difference**._ If no such `i` and `j` exists, return `-1`.
- https://leetcode.com/problems/maximum-difference-between-increasing-elements/description/

```python
def maximumDifference(self, nums: List[int]) -> int:
	
	minVal = float('inf')

	diff = -1
	for n in nums:
		if n < minVal:
			minVal = min(n,minVal)
		else:
			currDiff = n - minVal
			if currDiff > 0:
				diff = max(diff, currDiff)

	return diff
```
#### trick
- track the smallest number, and if the number is **STRICTLY** greater than the smallest num then we get the difference