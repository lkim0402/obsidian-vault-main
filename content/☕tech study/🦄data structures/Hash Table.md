- Motivation
	- Why not just have a gigantic array that stores everything? It turns out it requires LOTs of storage.
- Idea
	![[hash-general.png]]
	- Have a small array to store information
		- relatively small because it's relative to the number of keys that you might have
- **Hash function**
	- Converts the **key into an index**
		- Instead of directly using the key as index, we apply the hash function to the key first, and that hash function picks an index for us (like an index picker)
		- "scatters" the keys, behaves as if it *randomly assigned keys to indices*
	- Store key at the index given by the hash function
	- Do something if two keys map to the same place (should be very rare)
		- Collision resolution
- **[[Java Implementation of Hash Table]]**
	- [[HashSet (Java)]]
	- [[HashMap (Java)]]
# Collision Resolution
- (Separate) Chaining
	- Uses [[LinkedList]]
- Open Addressing
	- When a collision occurs, find an empty index and move to it to store data.
	- Space utilization is efficient, and it can be implemented with just one array.

