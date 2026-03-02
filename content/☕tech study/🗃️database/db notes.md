# 데이터 모델 검증
- join 할때
	- spring에서 dto를 사용해서 제어하기도 함
	- join은 필요한 정보만
- **성능 최적화 (3가지 방법)**
	- 인덱싱
	- 비정규화 (역정규화)
	- 퀴리듀닝
		- `WHERE` and `HAVING`
		- `LIMIT `and `OFFSET `사용
	- 애초에 성능은
# 역정규화
- 성능을 늘리기 위해 중복을 늘리는것?
- 1) 복잡한 join이 자주 일어날때, 2) 집계가 통계가 반복적으로 필요할 경우..
- 항상 비용 확인
- 쿼리문 안에 쿼리문 - sub query
	![[Pasted image 20250716133809.png]]
- 뷰
	- 가상 테이블
- update
	- 코드 레벨에서 대응해야함

# enity 연관관계 매핑 (jpa에서 젤 어려움)

**Lazy loading**
- JPA는 성능을 위해 연관된 엔티티를 **필요한 시점까지 데이터베이스에서 조회하지 않음**
- **`Order`를 조회하는 시점 (`em.find(Order.class, ...)`):** 
	- 이때 JPA는 `SELECT * FROM ORDERS WHERE ...` 쿼리만 실행
	- `Order` 객체는 가져오지만, 연관된 `member` 필드에는 **실제 `Member` 객체 대신 가짜 객체(프록시, Proxy)를 넣어둠**
	- 이 프록시 객체는 내부에 아무런 데이터도 가지고 있지 않아서, 디버거로 보면 필드들이 `null`처럼 보임
-  **`member` 객체를 실제로 사용하는 시점 (`order.getMember().getName()`):** 
	- `Order` 객체에서 `getMember()`를 호출하여 프록시가 아닌 실제 `Member` 데이터에 접근하려는 **첫 번째 순간**에, JPA는 그제야 `Member` 데이터를 가져오기 위한 쿼리(`SELECT * FROM MEMBER WHERE MEMBER_ID = ...`)를 데이터베이스로 보냄
	- 그리고 조회해온 실제 데이터로 프록시 객체를 채워 넣음
- member의 `get` 나 `find`직후
	- 이제 order안에서 member가 보임

STEPS 정리: 이 모든 과정은 1차 캐시 안에서 효율적으로 일어남.
1. **`em.find(Order.class, 1L)` 호출**
    - JPA는 DB에서 `Order` (ID: 1)를 조회합니다.
    - 가져온 `Order` 객체를 **1차 캐시에 저장**합니다.
    - 이때 `order.member` 필드에는 실제 `Member`가 아닌 **가짜 프록시 객체**를 채워 넣습니다.
2. **`order.getMember()` 등으로 프록시 초기화 시도**
    - JPA는 `member` 프록시를 초기화해야 함을 인지합니다.
    - 먼저 **1차 캐시**에서 해당 `Member` 객체(`MEMBER_ID`를 통해)가 있는지 확인합니다.
    - **(대부분의 경우 처음이므로) 1차 캐시에 없음:** DB에 `SELECT * FROM MEMBER WHERE ...` 쿼리를 보냅니다.
    - DB에서 가져온 `Member` 객체를 **1차 캐시에 저장**합니다.
    - `order` 객체의 `member` 프록시 필드를 1차 캐시에 저장된 **실제 `Member` 객체로 교체**합니다.
3. **이후 같은 `Member` 객체에 접근한다면?**
    - 만약 코드 뒷부분에서 `em.find(Member.class, ...)`로 같은 멤버를 다시 조회하려고 하면, JPA는 DB에 쿼리를 보내지 않습니다.
    - 이미 **1차 캐시에 해당 `Member` 객체가 존재**하므로, DB 접근 없이 캐시에서 바로 객체를 반환해 줍니다.


