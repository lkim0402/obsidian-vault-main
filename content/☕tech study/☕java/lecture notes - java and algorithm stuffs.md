- 모든 트리는 그래프임
- 모든 그래프가 트리는 아님
- 계층적 구조 - tree
	- 게층적은 뭔가 효울적으로 검색하기 위해서 쓰임
- 그래프
	- 거의 세상에서 모든 데이터는 그래프로 표현할 수 있음 (너무 많은 곳에서 사용됨)

- deep copy  vs shallow copy in reference variables
	- shallow copy
		- same memory
		- =
	- deep copy
		- `ArrayList<Integer> list2 = new ArrayList<>(list);`
		- `System.arrcopy(list, 0, list2, 0, list.size())`
			- src, srcPos, dest, destPos, length
		- When it doesnt work
			- list with > 1 dimensions (다차원배열) 
- tree traversal 면접때 나올 수 있음
- 해시 함수는 순수 함수여야만 함
	- 아니면 데이터를 복구 못함
	- deterministic
	- O(1)

- 백엔드
	- 대용량 데이터 다루는 법 
		- kafka - 실시간으로 대용량 데이터를 다룰 때
			- hadoop -> when u dont need live data


- 면접에서는 요즘 객관식으로 cs 질문 자옴..... 코테는 오히려 좀 쉬움
	- java에 대해선 좀 깊에 알아야함

- float vs double
- **정수형 표시가능 범위 다 외우기**
	- byte - 8 bits ($2^8$, or 256)
		- -128 ~ 127
	- short - 2^16
	- int 2^32
		- float (소수점 8자기)
	- long 2^64
		- double (소수점 15자리)
- **부동소수점**
	- 0.3 + 0.3 == 0.6 -> true
	- 0.3 + 0.3 + 0.3 == 0.9 -> false??
	- 실수를 쓸 때 Big Decimal 사용하기 <<<


generic 노션
1,2,4는 필수
3,5,6는 심화


# File I/O

- java의 정렬 알고지름
	- 2개있음 (원시, 참조형에 따라 다름)


대용량 데이터
- linkedlist잘 안씀, arraylist많이 사용함
	- 가장 선호되는건 아나 arraylist일것
- hash구조는 비교적 많이 쓰지 않음 (데이터가 많으면 해시 충돌이 일어날 가능성이 많기 때문에)
- 자료구조를 정할 때 infra 레벨에서 접근하는 경우가 많음
	- 대용량 데이터를 가지고 필터링을 해야함 (ex 쿠팡에 가위 검색)
		- 가ㅇ 까지만 쳐도 가위가 알아서 나옴
	- java/spring으로만 가지고는 완벽하기 구현 어려움
	- 검색 같은 경우 검색 엔진을 구현함 (ㅇ미 구현되어있는걸 씀) - elastic search 같은거

삽입/삭제가 잦고 위치가 중요한 경우
- linkedlist를사용할 수 있지만 주의해야함
	- 실제 실무에서는 CRUD의 R가 가장 중요하기 때문에
	- 항상 조회를 우선시해야함

trie
- 글자별 필터링때 유용함 - 키워드 자동완성
- 각 노드가 문자를 표현함
- trie를 쓰는 기술을 공부할 때 알아놔야함
- java에는 없어서 직접 구현해야함

# Sorting algorithms
정렬 꼭 알아야함
- data structure 에 따라서 어느 정렬 알고리즘
- stable vs not stable (safe and non safe)
- tim sort?
	- for python originally, also used in java 
	- merge (정렬X부분)+ insert (정렬된 부분)
- n log n 보다 빠른 sorting algorithm은 없음
- stable vs unstable
	- stable -> 순서를 유지함
	- 더 정확히 숙지하기
	- unstable
		- quick, heap, selection
- 알아야하는
	- quicksort
		- 무조건 알아야함
		- unstable
	- mergesort
		- ㄹ얘도 무조건...
		- 더이상 쪼갤 수 없을떄가지 쪼개고, 병합
		- 병합할때야 정렬이 이루어짐
		- stable
	- bubblesort (너무 기본이라 학습용임)
		- 생각보다 손코딩에서 많이 나옴
		- 그냥 뼛속부터 알아야함 - 종이에 대고 바로 쓸 수 있어야함
		- 가장 큰 데이터가 맨 뒤로감 
	- insertion sort
		- 외우기 ㅋㅋ
	- selection sort
		- 첨-끝까지 돌고, 가장 작은 숫자랑 swap
- 가능하면 best average worst 다 알고있어야 함
- counting sort
	- 각 값의 등장 횟수를 셈
	- 엄청 큰 단점: 정수만 가능 + 메모리 낭비
- radix sort
	- 자릿수별로 나누어 정렬
	- %10, then %100.... 
- heap sort
	- 천만건의 데이터에서 2번째로 큰 수 
- binary search
	- 정렬되어있어야함, 중복 없음

