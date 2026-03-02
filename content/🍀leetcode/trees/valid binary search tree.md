- https://neetcode.io/problems/valid-binary-search-tree/question
- this question kinda tripped me up

# solution
```python
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        
        def dfs(node, low, high):
            if not node: return True
            
            # check if the CURRENT node violates the low/high limits passed from above
            if not (low < node.val < high):
                return False
            
            # pass the CURRENT node's value as the new limit for children
            return dfs(node.left, low, node.val) and dfs(node.right, node.val, high)

        # initial call: use float('-inf') and float('inf') to represent no limits
        return dfs(root, float('-inf'), float('inf'))
```

- basically
	- If the tree is valid, you just keep narrowing the window tighter and tighter until you hit `None` (the leaves), at which point you return `True`.
- when you return from the `dfs` function call
	- ur passing the CURRENT node's value as the new lower/upper limit -> ur narrowing down the window. a valid bst will hold continue to be true despite the window is closed