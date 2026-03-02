Implement the [Least Recently Used (LRU)](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU) cache class `LRUCache`. The class should support the following operations

- `LRUCache(int capacity)` Initialize the LRU cache of size `capacity`.
- `int get(int key)` Return the value corresponding to the `key` if the `key` exists, otherwise return `-1`.
- `void put(int key, int value)` Update the `value` of the `key` if the `key` exists. Otherwise, add the `key`-`value` pair to the cache. If the introduction of the new pair causes the cache to exceed its capacity, remove the least recently used key.

A key is considered used if a `get` or a `put` operation is called on it.

Ensure that `get` and `put` each run in O(1)O(1) average time complexity.

# my solution
- **The concept- we use a hashmap + dll**
	- the cache (`hashmap`) 
		- key: stores the key
		+ value: the Node that has both key and value
		+ get/put key values in O(1) time
	+ the dll (`doubly linked list`)
		+ stores the Node that has both key and value, we use a ll to store values in time based order
		+ `Node left` (LRU) -> oldest one is the head
		+ `Node right` (MRU) -> newer ones are just added to the end
		+ we use a dll and not a sll  because removing a node from the middle of a list requires *updating the pointers of the previous node*
+ implementation
	+ `get(int key)`
		+ we just check the cache and get the Node (value of the key)
		+ if key doesn't exist, return -1
	+ `put(int key, int value)`
		+ if key exists
			+ (no need to check capacity coz we're just updating values)
			+ get the corresponding node, remove the node, update its `val` (`node.val = value)`, then add to the end of the dll 
		+ if key doesn't exist
			+ check capacity
				+ if full, remove  `left.next`  (so that there will be space left for new node)
			+ get the corresponding node, insert to the end of dll
```java
class Node { // for double linked list
    int key;
    int val;
    Node next;
    Node prev;
    
    public Node(int key, int val) {
        this.key = key;
        this.val = val;
    }
}

class LRUCache {

    int capacity;
    Map<Integer, Node> cache;
    Node left; // LRU
    Node right; // MRU

    public LRUCache(int capacity) {
        this.cache = new HashMap<>();
        this.capacity = capacity;

        // initializing dummy nodes
        this.left = new Node(0,0);
        this.right = new Node(0,0);
        this.left.next = this.right;
        this.right.prev = this.left; 
    }
    
    public int get(int key) {
        if (!cache.containsKey(key)) return -1;

        // remove Node and re-add to linkedlist (MRU)
        Node node = cache.get(key);
        remove(node);
        insert(node);
        return node.val;
    }
    
    public void put(int key, int value) {
        if (!cache.containsKey(key)) { // new element
            // 1) check capacity, 2) put new node
            if (cache.size() == capacity) {
                Node LRU = this.left.next;
                remove(LRU); // remove LRU
                cache.remove(LRU.key);
            }
            Node node = new Node(key, value);
            cache.put(key, node);
            insert(node);
            return;
        }
        
        // removing old node to dll
        Node node = cache.get(key);
        remove(node);
        // adding new node to dll
        Node newNode = new Node(key, value);
        cache.put(key, newNode);
        insert(newNode);
    }

    // ========= linkedlist remove, insert helper methods ==========

    public void remove(Node node) {
        Node next = node.next;
        Node prev = node.prev;
        prev.next = next;
        next.prev = prev;
        // node.prev = null;
        // node.next = null;
    }

    public void insert(Node node) {
        Node prev = right.prev;
        // left and node
        prev.next = node;
        node.prev = prev;
        // node and right
        node.next = right;
        right.prev = node;
    }
}
```
- In `remove(Node node)`
	- We don't need to set the `node.prev` and `node.next` as null because Java's garbage collection cleans these dangling pointers (its a disconnected node)
- `this.left` and `this.right`
	- they are dummy nodes, and all new Nodes will be in between them ( via `left.next` and `right.prev`)
	- `insert(Node node)` will always add a new node to `right.prev`
- Time Complexity
	- `O(1)` for each `put()` and `get()` operation