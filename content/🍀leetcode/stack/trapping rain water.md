https://youtu.be/ZI2z5pq0TqA?si=t51soFqCLRkrQiEH
- first hard leetcode problem😭



- `water[i] = min(left_max, right_max) - height[i]`
	-  calculate the maximum height from left and right , and subtract by current height

```python
Water:      0  2  4  0  0  0
Heights:   [4, 2, 0, 6, 1, 0]
			   L     R
maxL: 4
maxR: 6
min(maxL, maxR) = 4

Water:      0  2  4  0  3  0
Heights:   [4, 2, 0, 6, 2, 5]
			         L  R
maxL: 6
maxR: 5
min(maxL, maxR) = 5

Water:      0  0  0  0  
Heights:   [4, 2, 0, 0]
			L     R
maxL: 4
maxR: 0
min(maxL, maxR) = 0
```

- **why can we trust just one side** as the bottleneck
	- even tho the variable `maxR` has a starting value of 1, we technically don't know the true value for `maxR` (which is 3 for this example)
	- Whichever side has the smaller max, that's the side that can safely be used to calculate water at that moment
	- we only trust what we've already seen, thats the best guarantee on the left