# reverse the string
```python
def isPalindrome(self, s: str) -> bool:

	ans = ""

	for c in s:
		if c.isalnum():
			ans += c.lower()
	
	return ans == ans[::-1]
```
- easy
- time complexity O(n)

# using 2 pointers
#### my solution
```python
def isPalindrome(self, s: str) -> bool:

	start = 0
	back = len(s) - 1

	while (start < back):
		if not s[start].isalnum():
			start += 1
			continue
		if not s[back].isalnum():
			back -= 1
			continue
		
		if s[start].lower() != s[back].lower():
			return False

		start += 1
		back -= 1

	return True    
```
- time: O(n) since we go thru the entire array
- memory: O(1) since we dont use extra memory
- https://www.youtube.com/watch?v=jJXJ16kPFWg
	- neetcode made a separate `isalnum()` function using `ord`

```python
def alphanum(self, c):
	return (
		ord('A') <= ord(c) <= ord('Z') or
		ord('a') <= ord(c) <= ord('z') or
		ord('0') <= ord(c) <= ord('9') or
	)
```