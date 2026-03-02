
>[!what is it]
>**Garbage Collection** is the process of **automatically freeing up memory** that a program no longer needs.
>- Without cleanup, this unused memory leads to **memory leaks** and reduced performance
>- it's handled by [[Java Virtual Machine (JVM)|JVM]], so devs don't need to manually manage memory manually

- [notion 노트정리](https://www.notion.so/Garbage-Collection-GC-1fb096ba270c8173a8f9df66bbc1e804?pvs=4)
- 면접 단골임

# How it works
- Java creates objects in memory (on the heap).
- When an object is no longer referenced by any variable or part of the program, it becomes eligible for garbage collection.
- The Garbage Collector (GC) detects these unreachable objects and reclaims the memory they occupied
- When Does an Object Become Eligible for GC?
	- When the reference is explicitly set to null
	- When the reference variable goes out of scope

Example
```java
Person person = new Person();
person.setName("김코딩");
person = null;             // 참조 해제 → GC 대상

person = new Person();
person.setName("박해커");

person = new Person("김러키"); // 이전 인스턴스는 GC 대상
```
- When dealing with [[Java Collections Framework (JCF)|collections]],  `remove()` actually deletes the reference from the collection
- if you’re dealing with a **collection (like a list or map)** and you just set an element in it to `null` without removing it
	- the collection still **holds a reference** to that position which can cause memory inefficiency / logical bugs
# Characteristics
- Automatic & Non-deterministic
	- You don’t manually delete objects, JVM does it for u
	- You can't predict exactly when the GC will run, it runs on memory performance, system idle, etc. on many factors
- you *can* request it tho, using `System.gc()`
# Key Phases
> What the JVM actually does internally when it performs garbage collection: 2 step process
- Stop The World
	- JVM pauses all application threads to run GC, leaving only the GC thread(s) active 
	- The application resumes after GC completes
- Mark and Sweep
	- Mark Phase: The GC identifies which objects are still in use (by checking if they are still referenced).
	- Sweep Phase: The GC removes unreferenced objects and reclaims memory.
# Generational Architecture
>The JVM divides the heap (where objects live) into different regions based on **how long objects survive** in memory
- diagram
	![[generational.png]]
- young
	- Most objects are created and destroyed quickly in the Young Gen
	- Because many objects die quickly here, Garbage Collection happens frequently in this area. This is called Minor GC.
- old
	- Only objects that survive several GC cycles get moved to the Old Gen
	- Garbage Collection happens less often in the Old Gen but is more expensive in terms of time and performance. This is called Major GC