
>[!Queue ADT]
>A “First In First Out” (**FIFO**) collection of items → a method of how it stores things

|                  | Description                                                                                                                                                                            |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| What is it?      | A "First In First Out" (FIFO) collection of items                                                                                                                                      |
| What Operations? | - Enqueue: Add new item to the queue (back)<br>    - Dequeue: Remove “oldest” item from the queue (front)<br>    - IsEmpty: Indicate whether or not there are items still on the queue |
- A queue data structure represented as a "chain" of items
	- A "front" variable referencing the oldest items
	- A "back" variable referencing the most recent item
	- Each item points to the item enqueued after it

- 코테준비
	- message queue??
	- 안쇄대기열 (프로세스)
- 적용 사례
	- OS 내부
	- buffer 관리

# Queue Data structures
- [[LinkedList]]
- [[Circular Array]]
# Code
- `offer()`/`add()`: 데이터 삽입 (구분 필요)
- `poll()`/`remove()`: 데이터 삭제 및 반환 (구분 필요)
- `peek()`: 가장 앞의 데이터를 조회 (제거하지 않음)
- `isEmpty()`: 큐가 비어 있는지 확인
```java
public static void main(String[] args) {

    /* Queue */
    
    /* Queue 자체로는 인터페이스 이기 때문에 인스턴스 생성이 불가능하다 */
//		Queue<String> que = new Queue<>();		// 에러남

    /* LinkedList로 인스턴스 생성 */
    Queue<String> que = new LinkedList<>();

    /* 큐에 데이터를 넣을 때에는 offer()를 이용한다. */
    que.offer("first");
    que.offer("second");
    que.offer("third");
    que.offer("fourth");
    que.offer("fifth");

    System.out.println(que);

    /* 큐에서 데이터를 꺼낼 때는 2 가지가 있다
     * peek() : 해당 큐의 가장 앞에 있는 요소(먼저 들어온 요소)를 반환한다.
     * poll() : 해당 큐의 가장 앞에 있는 요소(먼저 들어온 요소)를 반환하고 제거한다.
     * */
    System.out.println("peek() : " + que.peek());		// first
    System.out.println("peek() : " + que.peek());		// first

    System.out.println(que);			//5개 다 나옴

    System.out.println("poll() : " + que.poll());		// first
    System.out.println("poll() : " + que.poll());		// second

    System.out.println(que);
}
```