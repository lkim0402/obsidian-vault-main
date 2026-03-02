```python
def threeSum(self, nums: List[int]) -> List[List[int]]:
	sortNums = nums.sort()
	ans = []

	for i,n in enumerate(nums):

		# if n > 0 then it means all nums are positive going forward
		if n > 0:
			break
		
		# removing duplicates
		if i > 0 and nums[i - 1] == n:
			continue

		l = i + 1 # excluding the 1st
		r = len(nums) - 1

		while (l < r):
			curSum = n + nums[l] + nums[r]
			if (curSum == 0):
				ans.append([n, nums[l], nums[r]])
				l += 1
				r -= 1
				"""
				[2,2,-1,-1,0]
					  ^
				"""
				while (l < r and nums[l] == nums[l-1]):
					l += 1

			elif curSum > 0:
				r -= 1
			else:
				l += 1
	
	return ans
```
- So the main point is the while loop inside the while loop
	- you need to check to make sure the `l` finally lands in a place where its not a duplicate
	- imaging keep moving `l` until its not a duplicate (check with previous elem)