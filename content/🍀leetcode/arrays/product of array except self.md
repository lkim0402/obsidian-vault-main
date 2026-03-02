```python
def productExceptSelf(self, nums: List[int]) -> List[int]:
	"""
		 1   2   3  4
	pre  1   1   2  6
	post 24  12  4  1

		 24  12  8  6
	"""

	pre = []
	post = []
	prefix = 1
	postfix = 1

	for n in nums:
		pre.append(prefix)
		prefix = n * prefix
	
	print(pre)

	for i in range(len(nums) - 1,-1, -1):
		post.insert(0, postfix)
		postfix = nums[i] * postfix
	
	print(post)

	ans = []
	for i in range(len(nums)):
		ans.append(pre[i] * post[i])
	
	return ans

```
- basically just understand the output answer, each element is just pre * post
### A better solution 
- this one doesn't use extra pre/post arrays

```python
def productExceptSelf(self, nums: List[int]) -> List[int]:
	"""
		 1   2   3  4
	pre  1   1   2  6
	post 24  12  4  1
		 24  12  8  6

		 1    2    3  4
	pre  24   12   8  6
	post = 24
	"""

	ans = []
	# post = []
	prefix = 1
	postfix = 1

	for n in nums:
		ans.append(prefix)
		prefix = n * prefix
	
	print(ans)
	
	for i in range(len(nums) - 1, -1, -1):
		ans[i] *= postfix
		postfix *= nums[i]
	
	print(ans)
	return ans
```