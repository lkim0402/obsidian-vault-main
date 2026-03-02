# brute force(what i did)
```python
def isValidSudoku(self, board: List[List[str]]) -> bool:
	seen = set()

	# iterating over rows
	for row in board:
		for num in row:
			if num == ".":
				continue
			if num in seen:
				return False
			seen.add(num)
		seen.clear()

	# iterating over cols
	for i in range(9):
		for j in range(9):
			num = board[j][i]
			if num == ".":
				continue
			if num in seen:
				return False
			seen.add(num)
		seen.clear()
	
	# iterating over boxes
	for square in range(9):
		seen = set()
		for i in range(3):
			for j in range(3):
				row = (square // 3) * 3 + i
				col = (square % 3) * 3 + j

				num = board[row][col]

				if num == ".":
					continue
				if num in seen:
					return False
				seen.add(num)
	return True    
```

## the trick
- divide the entire thing into 0-9 blocks, then calculate the corresponding rows and columns
	- `row = (square // 3) * 3 + i`
		- `(square // 3)`
			- 0 -> 0,1,2
			- 1 -> 3,4,5
			- 2 -> 6,7,8
	- `col = (square % 3) * 3 + j`
		- `(square % 3)`
			- 0 -> 0,3,6
			- 1 -> 2,4,7
			- 2 -> 3,5,8
- also remember to ignore `"."` so that they don't get added in the duplicates

```python
"""
    0     1     2
    0 1 2 3 4 5 6 7 8
0 0 [0]   [1]   [2]
  1
  2
1 3 [3]   [4]    [5]
  4  x(4,0)
  5
2 6 [6]   [7]    [8]
  7
  8

"""
```

# neetcode
```python
def isValidSudoku(self, board: List[List[str]]) -> bool:
	# each key value is a set
	rows = collections.defaultdict(set) # row num : { numbers in row }
	cols = collections.defaultdict(set)
	squares = collections.defaultdict(set)

	for r in range(9):
		for c in range(9):

			num = board[r][c]

			if num == ".":
				continue
			
			in_row = num in rows[r] 
			in_col = num in cols[c]
			in_square = num in squares[(r // 3, c // 3)]

			if ( in_row or in_col or in_square):
				return False
			
			# adding to dicts
			rows[r].add(num)
			cols[c].add(num)
			squares[(r//3, c//3)].add(num)
	
	return True
```
## trick
- have a collection to store repeated numbers separate for rows, columns, and squares
- `squares` 
	- key is a tuple, like (1,0) -> box at row 1 and col 0
	- `(r // 3, c // 3)` maps 0-8 to 0-2