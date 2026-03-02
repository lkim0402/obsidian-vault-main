(Medium)

Given an array of strings `strs`, group all _anagrams_ together into sublists. You may return the output in **any order**.

An **anagram** is a string that contains the exact same characters as another string, but the order of the characters can be different.

**Example 1:**

```java
Input: strs = ["act","pots","tops","cat","stop","hat"]

Output: [["hat"],["act", "cat"],["stop", "pots", "tops"]]
```

#### Answer
- `O(m * nlog(n))` : loop through `m` elements in the array and sort every string of length `n`. Then compare if they are equal to check if they are anagrams.
Better solution
- `O(m*n*26) = O(m*n)`: `n` is the avg length of a string.
	- `count = [0] * 26`
	- key:value
		- `[1,1,0.....0] : ["ab", "ba"]`
	- Use `ord()` to return the unicode

```python
def Solution:
	def groupAnagrams(self, strs: List[str]) -> List[List[str]]:

		# making the default a list so we don't encounter edge cases
		res = defaultdict(list) 
		
		for s in strs:
			count = [0] * 26 # a...z
			
			for c in s:
				count[ord(c) - ord("a")] += 1

			# res[count].append(s) => list cannot be keys!
			res[tuple(count)].append(s)
		
		return res.values()
```