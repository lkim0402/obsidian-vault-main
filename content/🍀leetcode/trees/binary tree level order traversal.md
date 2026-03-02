- https://neetcode.io/problems/level-order-traversal-of-binary-tree/question?list=neetcode150
- my first medium tree question 😭
# my solution - bfs
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if root is None: return []
        
        q = deque([root])
        res = []

        while q:
            sublist = []
            for i in range(len(q)): # pop everything from that level
                cur = q.popleft()
                sublist.append(cur.val)
                if cur.left: q.append(cur.left)
                if cur.right: q.append(cur.right)
            res.append(sublist)
        
        return res
            
```

- for some reasons this was easier than the easy questions??
- edge case
	- the `root` might be `null`!! -> we have to remember this
- time complexity
	- `O(N)`
- space complexity
	- `O(N)`
	- determined the maximum number of node in queue `q` at any single moment. the queue holds one complete level
	- so at the last level, it will hold approximately `N/2` nodes -> this is `O(N/2)` which simpliies to `O(N)`

# apparently there is a dfs solution
```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        res = []

        def dfs(node, depth):
            if not node:
                return None
            if len(res) == depth:
                res.append([])

            res[depth].append(node.val)
            dfs(node.left, depth + 1)
            dfs(node.right, depth + 1)

        dfs(root, 0)
        return res
```
- umm....wtf is this 😭(this solution messed me up but after long staring i think i got it lmao)
- basically....... we calculate the "depth" of each node and use that value as index for `res`

do a quick run through with:
```
    3      (Level 0)
   / \
  9   20   (Level 1)
```