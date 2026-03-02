- https://neetcode.io/problems/binary-tree-diameter/question
- how is this an "easy" question omgg 😭
# my attempt (wrong)
```python
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
    
    res = 0
    
    def dfs(root):
	    nonlocal res
	    
	    if not root:
		    return 0
		  
		 left = dfs(root.left)
		 right = dfs(root.right)
		 
		 # wrong from here
		 cur = 1 + leftMax + rightMax
		 res = max(res, cur)
		 return cur
		
	return res
```
- This code is returning the diameter to the parent, but the parent doesn't care about diameter
	- the parent only cares about the **max height** ... rn it returns a "V" shape instead of a path lol
# solution
```python
class Solution:
    def diameterOfBinaryTree(self, node: Optional[TreeNode]) -> int:
        res = 0

        def dfs(node):
            nonlocal res 

            if not node:
                return 0
            
            left = dfs(node.left)
            right = dfs(node.right)

            # get current diameter and update res
            res = max(res, left + right)
            return 1 + max(left, right)
        
        dfs(node)
        return res
```

- basically
	- we get the through path (diameter) of current `node`, which is left height + right height. this is also what we update in `res`
		- dfs (recursive) function returns the bigger height
	- we then need to return the current height - the length of the longest path from node `N` down to its deepest leaf
- `res = max(res, left + right)`
	- the reason why we update `left + right` and not `left + right + 1` is because we're getting the edges
# time complexity
- `O(n)`
	- the algorithm visits every single node exactly once, and in each node we do a constant number of operations