```python
def isValid(self, s: str) -> bool:
	stack = []
	parenSet = {"{":"}", "[":"]", "(":")"}
	i = 0
	if len(s) < 2: return False

	"""
	()[]
	({)} - false
	{{}}
	}{
	([{}])
	once its not in parenSet, we check if its the last elem in stack
	"""
	
	for c in s:
		if c in parenSet:
			stack.append(c)
		else:
			if stack and c == parenSet[stack[-1]]:
				stack.pop()
			else:
				return False
	return len(stack) == 0
 
```
- the trick is to check the last element of the stack & compare with current elem if its not `({[`
- 