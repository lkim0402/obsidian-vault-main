
>[!definition]
>A Red-Black Tree is a type of self-balancing [[Binary Search Tree]] (BST)
>- It keeps its **height balanced automatically** during insertions and deletions.
>  This balance **guarantees** that operations like search, insert, and delete take `O(log n)` time, **even in the worst case**.

- Java uses this Red Black Tree internally for [[TreeSet (Java)]] and [[TreeMap (Java)]]
# How does it maintain balance?
- Each node in the tree has an extra piece of information — a **color** — which is either **Red** or **Black**.
#### Rules
The tree follows some strict rules to maintain balance:
1. **Root node is always Black.**  
	- This ensures the tree starts balanced.
2. **All leaf nodes (the NIL or null nodes) are Black.**  
    - This is a technical detail that helps with uniformity in counting black nodes on paths.
3. **No two Red nodes can be adjacent.**  
    - A Red node cannot have a Red child. This prevents long chains of red nodes which could skew the tree.
4. **Every path from the root to a leaf must have the same number of Black nodes.**  
    - This guarantees the paths are roughly the same length, so the tree doesn’t become too tall.
#### Why these rules matter?
- These constraints prevent the tree from becoming unbalanced, which would make operations slower.
- The tree’s height is limited to about **`2 × log₂(n)`** (still logarithmic), so searching, inserting, and deleting are always efficient.