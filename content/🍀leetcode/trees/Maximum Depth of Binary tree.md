- https://neetcode.io/problems/depth-of-binary-tree/question
- ohhhh man i am in great danger i need to get good at this (and this is even an easy question am i cooked)
- also switching to python for an upcoming python interview

# solution 1 - dfs (recursion)
![[recursiondfs.png]]

```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))
```
- we look at the max of both subtrees

# solution 2 - dfs (iteration)
```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        stack = [[root, 1]]
        res = 0

        while stack:
            node, depth = stack.pop()

            if node:
                res = max(res, depth)
                stack.append([node.left, depth + 1])
                stack.append([node.right, depth + 1])
        return res
```
- using a stack
# solution 3 - bfs (iteration)
```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        q = deque([root])

        height = 0
        while q:
            for i in range(len(q)): # iterating by level
                node = q.popleft()
                if node.left: q.append(node.left)
                if node.right: q.append(node.right)
            height += 1
        return height
```

- my initial idea + attempt
- basically
	- the for loop in the middle - pull everything out in the queue (per level)
	- u have to use `range(len(q)` to hardcode (takes snapshot of `len(q)` at that time) -> we have to do this because `q` is changing