
# my attempt.. (worked!)
```python
class Solution:   
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        """
        go thru each node
        if node is equal to subroot node, do dfs on it
        """

        q = deque([root])
        while q:
            cur = q.popleft()
            if cur.val == subRoot.val:
                if self.dfs(cur, subRoot): 
                    return True
            if cur.left: q.append(cur.left)
            if cur.right: q.append(cur.right)

        return False

    def dfs(self, cur1, cur2):
        if not cur1 and not cur2:
            return True
        elif cur1 and cur2 and cur1.val == cur2.val:
            return self.dfs(cur1.left, cur2.left) and self.dfs(cur1.right, cur2.right)
        else:
            return False
```

- My idea was to just iterate using BFS (a stack), and if the current node's value is the same as the `subRoot`, then u perform DFS
	- so BFS + DFS
	- The DFS part is just checking if they are the same subtree
- Time complexity
	- `O(M * N)`
		- `O(M)` - inner `dfs` function
			- checks if 2 trees are identical. in the worst case it must visit every  node in `subRoot`
		- `O(N)` - the outer `isSubtree` function
			- checks the main tree with a queue. worst case it checks every node
# official solution
- DFS + DFS
```python
class Solution:   
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        """
        go thru each node
        if node is equal to subroot node, do dfs on it
        """

        if not subRoot: # empty trees are subtrees of all trees 
            return True
        if not root: # means we have a subRoot but no root -> alse!
            return False
        
        if self.dfs(root, subRoot):
            return True
        return self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)

    def dfs(self, cur1, cur2):
        if not cur1 and not cur2:
            return True
        elif cur1 and cur2 and cur1.val == cur2.val:
            return self.dfs(cur1.left, cur2.left) and self.dfs(cur1.right, cur2.right)
        else:
            return False

```

- The two base cases
	- `if not subRoot: return True`
		- checks if `subRoot` is empty.. that means we can just return `True`
	- `if not root: return False`
		- it means we havent found any equal subtree and we passed an empty `root.left` or `root.right`
		- then it means that in this `root.left` or `root.right` (which is empty) there is no more equal subtree in this path
- edge cases
	- `root` and `subRoot` are both `None`
		- they are in that specific order because if not:
			- Then `if not root: return False` would return `False`, but the application should actually return `True`
		- a null subtree is a subtree of another null subtree
	- `root` is `None` but `subRoot` exists