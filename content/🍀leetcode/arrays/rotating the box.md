https://leetcode.com/problems/rotating-the-box/description/

```python
def rotateTheBox(self, boxGrid: List[List[str]]) -> List[List[str]]:
	rowLength = len(boxGrid)
	colLength = len(boxGrid[0])

	# letting the stones fall
	for r in range(rowLength):
		i = colLength - 1
		for c in reversed(range(colLength)):
			# if stone found, replace with empty
			if boxGrid[r][c] == "#":
				boxGrid[r][c],boxGrid[r][i] = boxGrid[r][i], boxGrid[r][c]
				i -= 1
			# if obstacle found, move i pointer next to it
			elif boxGrid[r][c] == "*":
				i = c - 1
					
	# now rotating the boxgrid
	rotatedGrid = []
	rev = boxGrid[::-1]
	for i in range(colLength): # per col
		r = []
		for j in range(rowLength): # per row
			r.append(rev[j][i])
		rotatedGrid.append(r)
	
	return rotatedGrid
```
#### trick
1. shift the stones
	- go thru each row, in each row, iterate from the back
	- have 2 pointers: one for tracking the empty space `i`, one to iterate thru the row `c`
	- if stone found in `c`, then swap the position with `i` then move i
	- if there is an obstacle in `c`, then the `i` position is now `c + 1` because the rocks now can't move beyond the obstacle  
2. then rotate the box
	- basically the 1st column becomes the 1st row
	- so basically go thru the columns from the bottom