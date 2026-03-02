
>[!Thread]
>A **thread** is like a lightweight, independent path of execution inside your program
>- Thread is like the smallest unit of a process
>- Every Java program has at least **one thread**, called the **main thread**.

You can create **additional threads** to do tasks **in the background** while the main thread continues.

- core -> thread
- 코어수 스레드 수
- context switching
	- 작업환경을 돌아다님
- types
	- 물리 스레드
		- physical available threads in hardware
	- 논리 스레스
		- 해야할 일
		- 갯수에 제한이 없음
	- virtual thread (newer java)
		- no need to use thread pool
- thread pool
	- making and saving threads to be used later
- main function
	- main thread keeps running
	- 프로그램의 가장 큰 틀을 관리함
- multi thread
	- ㅈ바는 multi thread 환경을 지원함 (node doesnt)
	- 하나의 프로세스는 여러 thread를 가질 수 있음
		- 하나의 thread를 가지고 있는데, 여러 threadㄷ가 파생된거임!
- 쓰는 방법
	- 해야할 일을 지정하는 거임
	- 방법들
		- runnable interface (함수형)
		- thread클래스를 상속받은 하위 클래스에서 run
	- thread를 생성하는 resource가 들어감
		- 생성되고 들어가는 cost가 있음
- you can get name of the thread lmao
	- `thread.getName()`
	- also can give name to thread `.setName("thread1")`, but not used as much
- Thread.sleep
	- stopping the thread
- Thread는 나중에 무조건 이해해야함

![[Pasted image 20250605193725.png]]
# extend Thread
```java
class Task extends Thread {
    public void run() {
        for (int i = 0; i < 3; i++) {
            System.out.println("Running in: " + Thread.currentThread().getName());
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Task t1 = new Task();
        Task t2 = new Task();

        t1.start();
        t2.start();

        System.out.println("Main thread: " + Thread.currentThread().getName());
    }
}
```
- You can extend your class with `{java}Thread`
	- You can't extend other class
- you have to use the start method
	- creates a new thread for you, and calls the `run` method of the thread automatically
- If you want to pause ur thread (milliseconds)
	- checked exception
	- `{java}Thread.sleep(10)`

# implements Runnable
- There is no start method (only run)