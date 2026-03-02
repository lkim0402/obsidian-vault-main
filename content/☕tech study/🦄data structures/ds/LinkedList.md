- Can be used to implement multiple ADTs, including [[Queue]] and [[List]]
![[Pasted image 20250606181142.png]]
- Nodes
	- the front/back variables
	- **Head node**
		- The 1st node
	- **Tail node**
		- The last node
	- In memory
		- They are **NOT stored contiguously in memory**, instead each node has the **address** of its succeeding node
		- So when adding/deleting elements, its performance is much better than [[List#ArrayList|arraylists]] 
		- Since each node contains the data and links (references), the more nodes the more memory usage
	- Nodes that are not referenced anymore (in dequeue) are cleaned up by [[Garbage collection (GC)]]
- `O(n)` for traversal
#### In Java
```java
LinkedList<Integer> list = new LinkedList<>(List.of(1,2,3));
```
- Its possible to use `list.get()` to access an element, but its not recommended
	- Every find function you use for a LinkedList, you have to iterate through it
#### Types
- **singly linked list**
	- ``[Data | Next] -> [Data | Next] -> [Data | Next] -> NULL``
- **doubly linked list**
	- `NULL <- [Prev | Data | Next] <-> [Prev | Data | Next] -> NULL`
	- used in [[☕Java]]

| Feature                        | Singly Linked List                          | Doubly Linked List                            |
| ------------------------------ | ------------------------------------------- | --------------------------------------------- |
| **Node Structure**             | Each node has data + a pointer to next node | Each node has data + pointers to next & prev  |
| **Navigation Direction**       | One direction (forward only)                | Two directions (forward and backward)         |
| **Memory Usage**               | Less (only one pointer per node)            | More (two pointers per node)                  |
| **Insertion at Head**          | O(1)                                        | O(1)                                          |
| **Insertion at Tail**          | O(n) without tail pointer, O(1) with it     | O(1) if tail pointer is maintained            |
| **Deletion at Head**           | O(1)                                        | O(1)                                          |
| **Deletion at Tail**           | O(n) (must traverse)                        | O(1) if tail pointer is maintained            |
| **Deletion of Arbitrary Node** | O(n), need previous node                    | O(1), if node reference is given              |
| **Backward Traversal**         | Not possible                                | Possible                                      |
| **Ease of Implementation**     | Simpler                                     | More complex                                  |
| **Use Cases**                  | [[Stack]], simple list-based structures     | Deques, undo-redo systems, complex navigation |

# [[Circular Array]] vs LinkedList
| Feature                      | Linked List                                | Circular Array                                          |
| :--------------------------- | :----------------------------------------- | :------------------------------------------------------ |
| **Resizing Procedure**       | No specific resizing procedure             | More complex (requires resizing)                        |
| **Space Usage**              | Better for a large queue - uses less space | More space (linked list uses 2 things to keep track of) |
| **Indexing**                 | X                                          | Easy to do                                              |
| **Complexity of Operations** |                                            | More complex (resizing, modulus, etc.)                  |