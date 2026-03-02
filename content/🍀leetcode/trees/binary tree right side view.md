- https://neetcode.io/problems/binary-tree-right-side-view/question
# bfs solution (ez)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        if root == None: return []

        q = deque([root])
        res = []

        while q:
            qLen = len(q)
            last = None
            for i in range(qLen):
                cur = q.popleft()
                last = cur.val
                if cur.left: q.append(cur.left)
                if cur.right: q.append(cur.right)
            res.append(last)
        
        return res
```

# dfs solution (woah)
```python
class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        if root is None: return []
        res = []

        def dfs(root, lvl):
            if root is None:
                return
            
            if len(res) == lvl:
                res.append(root.val)

            dfs(root.right, lvl + 1)
            dfs(root.left, lvl + 1)

        dfs(root, 0)
        return res
```
- the point is that you have to **call the right side first**!!