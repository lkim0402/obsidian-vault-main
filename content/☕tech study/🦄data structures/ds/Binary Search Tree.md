
>[!definition]
>A Binary Search Tree (BST) is a special type of [[Binary tree]] 
>- the left child of a [[Node]] has a value less than the parent node’s value 
>- the right child has a value greater than the parent node’s value.

- Worst case to insert: `n`. 
	- In the worst case, if you insert elements into a BST in a sorted order (e.g., inserting sorted keys), the resulting tree can degenerate into a [[LinkedList]]. 
		- This happens when each new node is added as the right child of the previous node, forming a linear chain 
	- In such a degenerate tree, the height of the tree becomes` n−1` (where `n` is the number of nodes), and the time complexity for insertion becomes` O(n)` because you may need to traverse the entire height of the tree.
- Worst case to deleteMin: `n`. 
	- In the worst case, if the minimum element is at the leftmost leaf of the tree, you may need to **traverse the entire height of the tree** to find and delete the minimum element.
	- Similar to insertion, if the tree is degenerate (linear), the height is `n−1,` resulting in a worst-case time complexity of `O(n)` for deleteMin.
