- check if the tree is balanced
# my attempt? (works)
```python
class Solution:

    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        return self.dfs(root) != -1

    def dfs(self, root):
        if root == None:
            return 0

        leftMax = self.dfs(root.left)
        rightMax = self.dfs(root.right)

        if leftMax == -1 or rightMax == -1 : return -1
        
        if abs(leftMax - rightMax) > 1:
            return -1

        return 1 + max(leftMax, rightMax)

```
- my initial attempt was using a boolean separately `balanced = True`, then changing it when `if abs(leftMax - rightMax) > 1:` and somehow breaking out of the entre dfs loop.... 
	- which abs can also work. just have a global variable outside.  and instead of returning `-1` just set it to false in the if case.
- but i asked gemini lol and it suggested that i use a signal, sending `-1` if its unbalanced
- time complexity - `O(N)`
	- In the worst case (a balanced tree), the algorithm must visit every single node exactly once to verify the balance.
	- we do constant number of operations per node
- space complexity - `O(H)`
	- every time you call `self.dfs()`, the computer adds a "frame" to the call stack (this takes memory)
		- the space complexity is equal to the maximum height of the tree
	- 2 scenarios
		- worst case - `O(N)` because this happens in a skewed tree (A ->B->C->D)
		- best case - `O(log N)` when its balanced (tree is branched out widely & stack never gets deeper than `log n`)
# official solution