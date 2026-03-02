
![[Pasted image 20250606181153.png]]
- Indices - the front/back variables (index pointers)
- enqueue
	- `back = (back + 1)% length`
	- will go back to index 0 once it's over the array length
- dequeue
	- `front = (front + 1) % length`
	- will also wrap

# Circular Array VS [[LinkedList]]

| Feature                      | Linked List                                | Circular Array                                          |
| :--------------------------- | :----------------------------------------- | :------------------------------------------------------ |
| **Resizing Procedure**       | No specific resizing procedure             | More complex (requires resizing)                        |
| **Space Usage**              | Better for a large queue - uses less space | More space (linked list uses 2 things to keep track of) |
| **Indexing**                 | X                                          | Easy to do                                              |
| **Complexity of Operations** |                                            | More complex (resizing, modulus, etc.)                  |