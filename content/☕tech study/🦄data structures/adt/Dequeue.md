>[!Deque ADT]
>A “Double-Ended Queue” (**Deque**) is a collection of items where elements can be **added or removed from both the front and back** — combining features of both [[Stack|stacks]] and [[Queue|queues]].


- Data structures
	- [[Queue#LinkedList|Linkedlist - doubly linked list]]

# Code
```java
public static void main(String[] args) {

    Deque<Integer> deque = new ArrayDeque<>();

    // Deque에 데이터 추가
    deque.addFirst(1);     // 앞쪽에 추가
    deque.addLast(2);      // 뒤쪽에 추가
    deque.addFirst(0);     // 다시 앞쪽에 추가

    System.out.println(deque);

    // 데이터 조회
    System.out.println("peekFirst() : " + deque.peekFirst()); // 0
    System.out.println("peekLast() : " + deque.peekLast());   // 2

    // 데이터 제거
    System.out.println("removeFirst() : " + deque.removeFirst()); // 0 제거
    System.out.println("removeLast() : " + deque.removeLast());   // 2 제거

    System.out.println(deque);
}
```

#### Using it like a stack/queue
```java
public static void main(String[] args) {

    Deque<Integer> dequeAsStack = new ArrayDeque<>();

    // Stack처럼 사용 (LIFO)
    dequeAsStack.addLast(1);
    dequeAsStack.addLast(2);
    dequeAsStack.addLast(3);

    System.out.println(dequeAsStack.removeLast()); // 3
    System.out.println(dequeAsStack.removeLast()); // 2

    Deque<Integer> dequeAsQueue = new ArrayDeque<>();

    // Queue처럼 사용 (FIFO)
    dequeAsQueue.addLast(1);
    dequeAsQueue.addLast(2);
    dequeAsQueue.addLast(3);

    System.out.println(dequeAsQueue.removeFirst()); // 1
    System.out.println(dequeAsQueue.removeFirst()); // 2
}
```