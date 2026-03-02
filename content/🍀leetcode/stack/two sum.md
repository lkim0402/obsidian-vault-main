```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
	"""
	{index: number}

	check if target - n exists in numDict
	"""
	numDict = {}
	i = 0
	for n in nums:
		numDict[n] = i
		i += 1
	
	print(numDict)
		
	for i,n in enumerate(nums):
		num = target - n
		if num in numDict and i != numDict[num]:
			return [i, numDict[num]] 
```

- the key is to create a numDict that hashes index to number 
	- not vide versa coz `nums` can have duplicates