```python
def maxArea(self, heights: List[int]) -> int:
	"""
	get initial area (l, r pointer)
	move the shorter one

	[1,2,3] -> width = 2 (if u think like they are sticks)
	"""

	l = 0
	r = len(heights) - 1
	width = len(heights) - 1
	maxArea = 0

	while (l < r):
		left = heights[l]
		right = heights[r]
		smaller = min(left, right)
		area = smaller * width
		maxArea = max(area, maxArea) # storing the maximum area

		width -= 1
		if left < right:
			l += 1
		else:
			r -= 1
	
	return maxArea
```
- move the shorter one to account for the decreasing width
	- the area is constrained by the shorter height
	- since the width always decreases & we need to find the maximum area, we need to move the shorter line since moving the taller one won't help
- also, the width can just be `r - l`