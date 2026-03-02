- Data structures are essential for efficient data management and processing.
- Reasons for using data structures:
	- Enables **fast and accurate data retrieval**.
	- Increases the efficiency of **data insertion, deletion, and modification operations**.
	- Maximizes the efficiency of **memory and processing time resources**
# Types
#### Linear VS Non-linear

| Category                 | Description                                                                                                                                               | Connection Type                                        | Memory Structure                                                   | Traversal Method                                 | Key Examples                                     | Use Cases                                                                |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ | ------------------------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------------------------------ |
| **Linear Structure**     | A structure where data elements are arranged sequentially, with each connected to one previous and one next element (1:1).                                | 1:1 connection (one before and one after)              | Stored contiguously in memory or with simple links                 | Traversed from front to back or back to front    | Array, [[List]], [[Stack]], [[Queue]], [[Deque]] | Queue processing, function call stack, browser history management        |
| **Non-linear Structure** | A structure where data elements have **hierarchical** (Tree) or **complex** (Graph) relationships, with one node potentially having multiple connections. | 1:N or N:N connections (parent/child, neighbors, etc.) | Stored non-contiguously, relationships maintained via **pointers** | Requires DFS, BFS, or other traversal algorithms | Tree, Graph, Heap, Trie                          | Folder structures, organizational charts, pathfinding, database indexing |
#### Static VS Dynamic
| Category              | Description                                                                                           | Memory Allocation                       | Resizability                                | Performance Characteristics                                                   | Key Examples                                                                   | Use Cases                                                                                          |
| --------------------- | ----------------------------------------------------------------------------------------------------- | --------------------------------------- | ------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| **Static Structure**  | The size or shape of the data structure is determined before program execution and cannot be changed. | Fixed memory allocation at compile time | ❌ Not resizable (fixed size)                | Fast access speed (O(1) indexing), but risk of overflow if size exceeds limit | Array                                                                          | Fixed-size data handling, array-based operations, fixed-length buffers                             |
| **Dynamic Structure** | The size or shape can be changed flexibly during runtime depending on the amount of data.             | Dynamic memory allocation at runtime    | ✅ Resizable (elements can be added/removed) | More memory-efficient, but slightly slower due to pointer-based access        | [[Queue#LinkedList\|LinkedList]], [[List#ArrayList\|ArrayList]], HashMap, Tree | Real-time input processing, systems with frequent data insertion/deletion, variable-length buffers |

# When choosing a DS
#### Based on data 
|**Data Characteristic**|**Suitable Data Structures**|
|---|---|
|Large-scale data processing|`ArrayList`, `LinkedList`|
|Fixed-size data|`Array`|
|Frequent insertions/deletions|`LinkedList`|
|Need to maintain sorted order|`TreeMap`, `TreeSet`|
|No need for sorting|`HashMap`, `HashSet`|
#### Types and frequency of operations
|**Operation Type**|**Suitable Data Structures**|
|---|---|
|Frequent search operations|`HashMap`, `TreeSet`|
|Frequent insert/delete operations|`Stack`, `Queue`, `LinkedList`|
#### Based on memory
|**Data Structure**|**Memory Characteristics**|
|---|---|
|**Array**|Stores only data → memory-efficient|
|**Linked List**|Stores data + pointers → higher memory consumption|
|**Hash-based**|Provides fast lookup speed → incurs memory overhead|