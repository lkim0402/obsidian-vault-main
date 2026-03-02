
>[!Tree ADT]
>A **hierarchical** collection of elements called **nodes**, where each node may have **zero or more children**, and there is one unique **root** node — used to represent data with a branching structure.

- Some rules
	- It MUST have 1 parent
		- If it has 2 or more, its considered as a [[Graph]]
	- if a node has a child it is automatically a parent
		- all nodes except the leaves are parents
- Usage
	- [[DOM (Document Object Model)]]
# Terminology
- **Root Node:** The topmost node of a tree
- **Child Node:** A node that branches from another (a descendant)
- **Parent Node:** A node that has one or more child nodes
- **Sibling Nodes:** Nodes that share the same parent
- **Leaf Node (Terminal Node):** A node with no children
- **Internal Node:** A node that is not a leaf (has at least one child)
- **Subtree:** A tree formed from a node and all its descendants
- **Level:** The distance from the root (the root is at level 0)
- **Degree:** The number of edges (children) a node has
- **Depth:** The number of edges from the root to a specific node
- **Height:** The maximum level (depth) in the tree
# Tree Data structures
- [[Binary tree]]
- [[Binary Search Tree]]
	- [[AVL Trees]] (self-balancing BST)
- [[B Trees]] (multi-way balanced tree)
- [[Heap]]
	- [[Binary Heap]]
- [[Tries]] (specialized)
#### Java-Specific Implementations
> TreeSet and TreeMap are balanced tree-based implementations of Set and Map in Java.
- [[TreeSet (Java)]]
- [[TreeMap (Java)]]
# Tree traversal
- **Pre-order** (전위 순회) 
	- Root > Left > Right
	- Always visit the root node first 
- **In-order** (중위 순회)
	- Left > Root > Right
	- In the case of a [[Binary Search Tree]], in-order traversal extracts the data in an ascending sorted form.
- **Post order** (후위 순회)
	- Left > Right > Root
	- Suitable for tasks requiring a child → parent flow
		- ideal for situations where lower-level elements must be processed before their higher-level counterparts
		- well-suited for operations like memory deallocation, file deletion, or directory cleanup, where you need to handle subordinate items before their parents
- **Level order** (레벨 순서 순회) 
	- We traverse each level of the tree from top to bottom, and from left to right, starting from the **root**.
	- It's implemented using a **[[Queue]]**. 
	- This method is suitable when you need to find the distance between nodes, the shortest path, or require hierarchical access.
	- [[BFS]]

- DFS/BFS는 아주 그냥 코딩테스트 단골임
	- 딱 보자마자 그래프 순회네? 바로 튀어나와야함 ㄷㄷ