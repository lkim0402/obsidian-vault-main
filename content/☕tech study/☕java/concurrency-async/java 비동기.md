- 쓰레드 무조건 알아야함
	- 일군
- 이제부터는 심화

젤 중요한건 비공기 vs 동기
- 이걸 잘 이해하는것부터 시작이다

- 동기/비동기 vs 블로킹/논블로킹

- 동기
	- 한번에 하나의 일을 붙잡고 있는거
	- 기다리는거
	- 한줄한줄 차례대로
- 비동기
	- blocking and non blocking과 좀 다름
	- 기다리지 않음
	- 남한테 맡기고 나머지 일 수행
	- 대부분 더 빠름
		- 근데 연산작업을할때 동기처리가 더 빠를 수 있음

- 블로킹
	- 작업을 끝낼때까지 제어권을 반환하지 않음
- 논블로킹
	- 호출 즉시 제어권을 반환
	- 일군이 좀 더 똑똑해짐

노드js -> 단일스레드
- 다른 기술과 차이점 알아두면 좋음

main method -> mainThread
- entrypoint를 실행할떄 mainthread가 있는거임
- 새로운 스레드는 mainthread에서 파생되서 나옴

cpu에 있는 스레드 -> 논리 스레드?
- jvm 에서 설명하는 스레드는 논리 스레드임

- 컴퓨터 부품

- connection pool
	- default: hikaripool
- 스레드를 사용하면 만들고 해제해야함 - cost가 많이 들어감
	- 비용 - 스레드를 넘나들때 context switching 비용이 든다
- 매번 스레드를 생성하지 않아도 되는 threadpool
	- 새로운 스레드가 필요할때 요청만 하면 됨
	- 근데 갯수가 있음
		- 그래서 대기 공간이 있음


- thread
- threadpool
- 동기/비동기 차이
- 블록/논블록킹 차이

---
SQL

1. MySQL 아키텍처에 대해 설명해주세요
- 쿼리 실행 과정을 설명
	- client -> sql 쿼리
	- query cache (이전에 실행한적 있는 요소들을 여기서)
	- cache에 없으면 parser
		- tree 구조로 파싱
	- 전처리기 (preprocessor)
		- 검증 -> 실제 테이블? 문법 오류?
	- optimizer
		- 최적의 쿼리 
		- 아웃풋: 쿼리 실행 계획 => 쿼리에 대한 계획
		- explain 명령어?
	- query 실행 엔진
		- 쿼리 실행 엔진 (전 단계)
	- **storage engine** 에게 API요청을 보냄
		- 대표적으로 MySQL의 InnoDB, 마이아쌈?
		- MVCC, ACID, LOCK 등 각 처리는 storage engine안에서 처리가 됨
		- 여기서 처리를 해서 데이터로 보내줌
![[Pasted image 20251016201937.png]]

- 아키텍쳐
	- parser
	- 전처리기
	- query 실행엔진
	- Optimizer
	- storage engine
	- connector - 별도의 api와 연결시켜주는 부분

2. 트랜잭션 ACID에 대해 설명해주세요.
	- RDB를 사용하는 가장 큰 이유가 ACID임
	- Atomicity - 원자성
		- commit, rollback
	- Consistency - 일관성
		- 유효한 상태 보장
	- Isolation - 격리성
		- 트랜색젼 격리 수준 4단계 
			- 완전 보장은 가장 높은 수준의 Serializable
			- repeatable read, read committed, read uncommitted
		- 격리 수준이 높을 수록 성능이 떨어짐
	- Duration
3. MVCC란 무엇이고 왜 사용하나요?
	- multi version concurrency control
	- rdb에서 제공
	- 성능 저해하지 않는 상태에서 데이터 정합성이 깨지지 않도록 최대한 보장해주는 특성
	- 락을 사용하지 않음
	- 트랜색션 격리 수준에 따라서 조금 다름
		- repeatable read, read committed
4. 낙관적 락과 비관적 락의 차이점은 무엇인가요?
	- 낙관적
		- 락 없이 성능도 어느정도 보장하고, 데이터 정합성도 보장
		- 충돌이 없을걸 가정
	- 비관적
		- 성능 저하 발생, 근데 데이터 정합성 보장
		- 충돌이 일어날걸 가정해서 락을 걸로 시작
	- 락
		- 분산 락
5. 쿼리가 느리면 어떻게 개선하실건가요?
	- (많이 물어봄)
	- ORM 을 사용했을때는 쿼리 성능을 많이 생각안해볼수있는데, 데이터가 많아지면 이건 그냥 기본 역량으로 알고있어야함
	- **답이 어느정도 정해져있음!!**
		- **느리면 90%는 인덱스가 없는 경우임**
			- index - B-tree 구조의 인덱스.
				- (이거 그냥 공부해야함 ㅅㅂ)
6. 클러스터드 인덱스와 논클러스터드 인덱스의 차이점에 대해서 설명해주세요.

네트워크보다 DB부터 공부를!


---

event looop
- 하나의 스레드가 많은 일을

# 비동기 처리의 구현 메커니즘 소개
(비동기 처리의 개념과 필요성)
- 스레드 기반 비동기 처리 - 스레드의 갯수를 늘리는것, 스레드풀로 효율적인 관리
- 이벤트 루프 기반 비동기 처리 - 하나의 스레드가 왔다갔다 여러개 작업
	- node js 는 기본적으로 적용됨, 하지만 spring mvc는 설정해주어야함
	- web flux -> RxJava를 보고, project reactor, then web flux (spring mvc를 쓰지 않음 - 요청에 따라서 서브릿사용안하게됨)
	- 우리한텐 해당사항이 크지 않음
- 메세지 기반 비동기 처리
	1. producer -> message queue -> consumer
		- 메시지 큐 기반 (중간단계)
			- 서비스를 비동기로 처리, 주문/결제 서버가 따로 있다면?
			- consumer는 기다리고있다가 메세지가 큐에 들어왔을 때 메세지를 가져감
			- 메시지 관리, 그리고 수신자에게 보냄
			- 요청과 응답을 단절
		- kafka, rabbitmessage q
		- 성능을 바라보진않음. 
	2. pub sub << 꼭 알고있어야함 (많이 쓰임)

firebase cloud messaging
- 알람 처리 어어어엄청 쉽게 할 수 있음
- 앱개발할때


# 멀티스레드 프로그래밍의 위험성
- 백엔드엔지니어라면 무조건!!!!!!!!!!! 알아야함
- race condition, dead lock <<<<<< 외우자 그냥... 면접때 준비
- race condition
	- 
- deadlock

future에서 get 알때 블로킹 <<< ?
- future에서 데이터를 가지고 올때 블로킹이 걸린다

- callable안에는 레퍼런스 타입만 가능

개발자라면 정렬 필요한거 알아야함
- 최소한 quick sort, merge sort 알아야함.....................하......알아야할게 좀 많다 ^^
- 반할정복 -> 이건 그냥 개발자의 심장에 있어야함 ㅋㅋㅋㅋ

CompletableFuture.completedFuture("이미 완료된 값");
- 주로 모킹할때 사용됨

executor 직접 사용하면 꼭 shutdown으로 정리해주어야함


---

# 예외처리
```java
import java.util.concurrent.*;

public class BasicExceptionExample {
    public static void main(String[] args) {
        try {
            CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
                System.out.println("작업 시작: " + Thread.currentThread().getName());
                throw new RuntimeException("작업 중 오류 발생!");
            });

            System.out.println("메인 스레드는 계속 실행됩니다.");

            future.join(); // 비동기 작업의 결과를 기다림
        } catch (Exception e) {
            System.out.println("catch 블록 진입: " + e.getClass().getSimpleName());
            e.printStackTrace();
        }
    }
}
```
- `join()`이 없으면 예외확인을 못함 (그냥 뭍힘)
- wrapping 되어서 completion exception으로 나옴


- AOP 객체 패턴 정확히 이해하기


- DI/IoC
- AOP
- PSA

async
- proxy 내부적으로 사용 -> aop사용한다는것임
- 이때 발생하는 문제 (중요함)
	- **self invocation**
	- 클래스에 다른 메서드를 호출하면 proxy호출이 안됨!
	- aop를 사용하는 또라는 어노테이션 -> @Transactional

- transactional을 클래스 위에 붙히면?
	- private 메서드만 됨
	- public은 안됨 -> self invocation
		- transactional은 spring aop에 적용함, 동적 안됨
		- @async도 클래스에 적용하면 public method에만 가능함
https://calm-individual-12a.notion.site/Async-291c6b709828814ea9f3fdd19a4d19e1


![](https://calm-individual-12a.notion.site/image/attachment%3A0c9f0561-01bd-42cd-b043-4bd07a5a09d8%3A%E1%84%83%E1%85%A1%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%85%E1%85%A9%E1%84%83%E1%85%B3_(1).svg?table=block&id=291c6b70-9828-81e5-ae50-d903491ad588&spaceId=8d720266-d651-4530-b3e3-47e27aa42b6c&userId=&cache=v2)
비공기 스레드에서 예외가 발생한다고 해서 트랜색션 스레드가 롤백되지 않음!!!!!

**@Async 제약사항 핵심 요약**

- `@Async`는 **Spring 프록시 기반**으로 동작하므로 프록시를 반드시 거쳐야 함
- **자가 호출(Self Invocation)** 은 프록시를 거치지 않으므로 비동기 처리 불가
- **public 메서드만** 비동기 처리 가능
- **트랜잭션(@Transactional)** 은 다른 스레드로 전파되지 않음
- Bean 분리 또는 ApplicationContext를 통한 프록시 호출이 가장 안전한 방법


spring boot 3버전 -> spring 6버젼 이상

https://calm-individual-12a.notion.site/Async-291c6b7098288131af04cc9f54ddc31b



전공자라면 mb kb 걍 다 외워야한다...

|개념|설명|예시|
|---|---|---|
|비동기 (Asynchronous)|요청을 보낸 후 결과를 기다리지 않고 다음 작업을 수행|콜백, Future, Promise 등|
|논블로킹 (Non-blocking)|I/O 작업 중 스레드가 대기하지 않음|NIO, Event Loop 기반 I/O|

---

cold starter - 최조 데이터 fetch이 db까지 가야하는것 (캐시가 아니라)