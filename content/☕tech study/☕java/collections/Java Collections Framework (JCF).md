
>[!Jcf]
>A set of classes and interfaces that implement **commonly reusable data structures**. It provides a standardized architecture for storing and manipulating groups of objects.
>- Related: [[🦄Data Structures]], [[Abstraction - Classes & Interfaces|Abstraction - Classes & Interfaces in Java]]

- **Flexible Size**: Unlike standard arrays, which have a fixed size upon creation, collections can dynamically grow and shrink as you add or remove elements.
- **`java.lang`**: This package is automatically imported into every Java file. 
	- It contains fundamental classes like `Object`, `String`, `System`, etc., but not the Collections Framework, which is in `java.util`.
# Hierarchy
```
		Collection
			|
+-----------+-----------+
|           |           |
List       Set        Queue
```
- _Note: `Map` is also part of the JCF but does not inherit from the `Collection` interface._

#### `Collection` Interface
- This is the **root interface** for most collections. 
- It defines basic operations like adding (`add()`), removing (`remove()`), checking size (`size()`), and clearing (`clear()`).
#### `List` Interface
- An **ordered** collection (sometimes called a sequence) that **allows duplicate elements**. Elements can be accessed by their integer index.
- [[ArrayList in Collections|ArrayList]], [[ArrayList|ArrayList (DSA)]]
- [[LinkedList]]
#### `Set` Interface
- [[Java Implementation of Hash Table#HashSet| HashSet in Collections|HashSet]]
- [[TreeSet (Java)|TreeSet]]
#### `Queue` Interface FIFO
A collection used to hold elements prior to processing. Besides basic `Collection` operations, queues provide additional insertion, extraction, and inspection operations. Typically, they follow a **First-In, First-Out (FIFO)** order.
- [[Priority Queue]]
- ArrayDeque
#### `Map` Interface 🗺️ (not in the hierarchy but part of Collections)
- [[Hash Map (Java)|HashMap]]
- [[TreeMap (Java)|TreeMap]]



>[!note]
>- **Collection vs. Collections**:
>	- `Collection` (with a capital 'C') is an _interface_ (`java.util.Collection`) that is the **root** of the collection hierarchy.
>	- `Collections` (with an 's') is a _utility class_ (`java.util.Collections`) that provides static methods for operating on or returning collections.

- Read
	- [🧱 Java Collections Framework 종류 💯 총정리](https://inpa.tistory.com/entry/JCF-%F0%9F%A7%B1-Collections-Framework-%EC%A2%85%EB%A5%98-%EC%B4%9D%EC%A0%95%EB%A6%AC#map_%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4)
