Given the `root` of a binary tree, return _the inorder traversal of its nodes' values_.

**Example 1:**
**Input:** root = `[1,null,2,3]`
**Output:** `[1,3,2]`
**Explanation:**
![](https://assets.leetcode.com/uploads/2024/08/29/screenshot-2024-08-29-202743.png)

**Example 2:**
**Input:** root =` [1,2,3,4,5,null,8,null,null,6,7,9]`
**Output:** `[4,2,6,5,7,1,3,9,8]`

# Solution
```java
/**
 * Definition for a binary tree node.

 *  Left -> Root -> Right

 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
import java.util.Stack;

class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        return traversal(root, new ArrayList<>());
    }

    public List<Integer> traversal(TreeNode root, List<Integer> visited) {
        TreeNode cur = root;

        if (cur == null) {
            return visited;
        } 

        traversal(cur.left, visited);
        visited.add(cur.val);
        traversal(cur.right, visited);

        return visited; // because visited is a reference type

    }

}
```

- Let recursion handle it for you!
	- The problem was that i was thinking of everything too complex when i just needed to simplify everything