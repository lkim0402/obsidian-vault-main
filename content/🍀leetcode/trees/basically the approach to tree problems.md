- GENERAL IDEA
	1. Find one or more base cases
	2. Call the same function on the left subtree
	3. Call the same function on the right subtree
	4. Join the results

# sum of elements
```python 
def sum_elements(node):
	if not node:
		return 0
	leftSum = sum_elements(node.left)
	rightSum = sum_elements(node.right)
	return node.val + leftSum + rightSum 
```
# maximum element
```python 
def max_element(node):
	if not node:
		return float("-inf")
	leftMax = max_element(node.left)
	rightMax = max_element(node.right)
	return max(node.val, leftMax, rightMax) 
```
# tree height
```python 
def get_height(node):
	if not node:
		return 0
	leftHeight = get_height(node.left)
	rightHeight = get_height(node.right)
	return 1 + max(leftHeight, rightHeight) 
```

# exists in tree
```python
def existsInTree(node, val):
	if not node:
		return False
	else:
		inLeft = existsInTree(root.left, value)
		inRight = existsInTree(root.right, value)
		return node.val == val or inLeft or inRight
```