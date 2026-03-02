
- monotonic decreasing stack

```python
 def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
	ans = [0] * len(temperatures)
	stack = [] # holds pairs: [temp, index]
	
	for i,t in enumerate(temperatures):
		while stack and t > stack[-1][0]:
			temp, ind = stack.pop()
			ans[ind] = (i - ind)
		stack.append([t,i])
	
	return ans
```

the trick
- you basically keep track of the indices & store them as a pair
- while loop
	- you pop the stack while u find a bigger number
	- `ans[ind] = (i - ind)`
		- bigger number's index - popped element's index 
- time complexity - O(n)
	- technically o(2n) 
		- Each of the `n` elements is **pushed once** ➝ O(n)
		- Each of the `n` elements is **popped at most once** ➝ O(n)