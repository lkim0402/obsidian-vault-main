
>[!Dictionary ADT]
>A “First In First Out” (**FIFO**) collection of items → a method of how it stores things

|                  | Description                                                                                                                                                                            |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| What is it?      | A "First In First Out" (FIFO) collection of items                                                                                                                                      |
| What Operations? | - Enqueue: Add new item to the queue (back)<br>    - Dequeue: Remove “oldest” item from the queue (front)<br>    - IsEmpty: Indicate whether or not there are items still on the queue |


- Data structures
	- BST
	- Tries
# Binary Search Trees (BST)
# Tries
>[!Tries]
>A trie is a tree-based data structure used primarily for efficient storage and retrieval of string-keyed data.

- Has regular tree components.
- Each branch stores part of a key.
- The key for a node is represented by the path from the root to that node.
- Nodes store the value corresponding to the key.

- A simple implementation of a Trie node
```java
TrieNode {
	int value;
	Map<Character, Node> children;
}
```