
| ADT                                                                                                                              | Data Structure                                                                                               | Algorithm                                                                                          | Implementation of a data structure                                               |
| -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Defines behavior and operations (what it does not how)<br>	- Like an [[Abstraction - Classes & Interfaces#Interface\|interface]] | A **precise description of how to make the operations happen**; Higher level than programming language level | The **step-by-step logic** for **performing operations on the ADT**, independent of code           | **Programming language specific**; How you can do the data structure **in code** |
| **Queue**  <br>- Follows **FIFO** (First-In, First-Out)  <br>- Operations: `enqueue`, `dequeue`, `peek`, `isEmpty`<br>           | Linked List                                                                                                  | **To enqueue an element into a queue:**<br>1. Create a new node with the given value.  <br>2. .... | `enqueue(x){ last = new Node(x)...`                                              |
| **List**  <br>- An **ordered** collection of elements  <br>- Operations: `add`, `remove`, `get`, `size`, `contains`              | ArrayList                                                                                                    | **To remove an element from a list**:<br>1. Search for the element in the list                     | `remove { list.remove(x) };`                                                     |
# Abstract Data Type (ADT)
>[!main idea]
>_Only **behavior** is defined, not implementation_

- Mathematical **description** of a “thing” with a set of **operations** on that “thing”
	- What operations can be performed on data, but no specifications on how these operations are implemented
	- Ex) List → An ordered collection of things with possible repetition. Operations include adding, checking length, etc.
		- In [[☕Java]], an [[ArrayList in Collections]] (a data structure) is an implementation of a List 
			- `{java icon} ArrayList implements List<E>`
- Three Descriptors:
    1. Operation (e.g. queue has `add()` and `remove()` operations)
    2. Behavior (e.g. queue has FIFO behavior while stack has LIFO behavior)
    3. Data Type (e.g. queue can store any type of element)
 - [[Abstraction - Classes & Interfaces#Interface|interface]]
# Data structure
>[!Main idea]
>_Concrete **implementation** of an ADT that determines how data is organized and manipulated_

- A specific **organization of data** and family of **algorithms** for implementing an ADT
	- A specific way of **how to make the ADT happen**
- Concerned w/ low-level details of organizing and storing data efficiently in memory
- Ex) ArrayList → You can use the list operations
- Kind of like the small things that make up the ADT
# Algorithm
- A high level, language independent description of a step-by-step process; different from a function
- Algorithms are adapted into different languages of your choice → they can be in **plain english**, **pseudocode**, etc
# Implementation of a data structure
- A specific implementation in a specific language
- **Actually making the ADT happen**
- Ex) An ArrayList could be implemented in different languages ([[☕Java]], [[C++]], [[Python]], etc)