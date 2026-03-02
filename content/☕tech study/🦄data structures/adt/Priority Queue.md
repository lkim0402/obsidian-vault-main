
>[!Queue ADT]
>- A collection of items and their” priorities”
> - Allows quick access/removal to the “top priority” thing

|                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| What is it?      | A collection of items and their” priorities” which allows quick access/removal to the top priority thing                                                                                                                                                                                                                                                                                                                                                                                                                       |
| What Operations? | `insert(item, priority)`<br>    - Add a new item to the PQ with indicated priority<br>    - Usually, smaller priority value means more important<br>`deleteMIn`<br>    - Remove and return the “top priority” item from the queue<br>    - When removing something from the queue, the thing we will remove is the top priority thing<br>    - The thing with the top priority is the one whose priority value is sort of the smallest value<br>`Is_empty`<br>    - Indicate whether or not there are items still on the queue |

- Note: the “priority” value can be any type/class so long as it’s comparable (you can use “<” or “`compareTo`” with it)
- We remove elements in order of their priority
	- By convention, smaller numbers mean high priority
- Real life analogy
	- Emergency room at the hospital
    - Whoever needs service first, receives service first
# Thinking through implementations
![[pq-implementations.png]]
> _Note: Assume we know the maximum size of the PQ in advance_
- Unsorted array
    - Worst case to insert - 1
	    - you just need to stick it in the next available spot in the array (using some variable to tell the last index used)
    - Worst case to deleteMin - n
	    - worst case is when you need to look at everything
- Unsorted [[LinkedList]]
    - Worst case to insert - 1
	    - Add to beginning or end, doesn’t matter it’s always once (u have front/back nodes)
- Sorted array
    - Worst case to insert - $n$
	    - we shift the elements and make room for new element which makes it linear
    - Worst case to deleteMin - $n$ 
	    - due to same reason, but could also be $1$ if it’s a [[Circular Array]] 
		    - When you delete the minimum element, you can simply move the index or pointer to the next element in the circular order. There's no need to shift all elements, and the operation can be done in constant time $O(1)$.
- Sorted [[LinkedList]]
    - Worst case to insert - $n$
	    - you can’t skip or jump in the middle
    - Worst case to deleteMin - 1
	    - Since it’s already sorted, simply update the head pointer to point to the next element
- [[Binary Search Tree]]
- [[Binary Heap]] - has the best insert/delete for PQ
	- Some observations ⇒ a tradeoff
	    - When we had something unsorted
	        - Inserting was easy since it didn’t matter where something went
	        - But it was hard to find the minimum thing since there was no order
	        - And vise versa
	    - But the binary heap manages to do well on both!
	        - Kind of sorted → little bit of order but not total order