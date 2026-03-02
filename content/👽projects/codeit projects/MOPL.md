# Websocket + watchingsession
- 첫 콘텐츠 클릭 (WebSocket 연결 없음)
```
1. GET API 호출 (빠름) 
   → 현재 시청자 목록 조회
   → UI에 표시
   
2. WebSocket 연결 시작 (느림)
   → TCP 핸드셰이크
   → STOMP 연결
   
3. WebSocket 연결 완료
   → SUBSCRIBE /sub/contents/{contentId}/watch
   → handleSessionSubscribe 호출
   → joinSession() → 세션 생성 (DB 저장)
   → Redis Pub/Sub으로 WatchingSessionChange 브로드캐스트
   → UI 업데이트 (내가 입장한 것 표시)
```
-  다른 콘텐츠 클릭 (WebSocket 이미 연결됨)
```
1. 기존 콘텐츠 UNSUBSCRIBE (빠름)
   → handleSessionUnSubscribe 호출
   → leaveSession() → 세션 삭제 (DB 삭제)
   → Redis Pub/Sub으로 WatchingSessionChange 브로드캐스트
   → UI 업데이트 (내가 퇴장한 것 표시)

2. 새 콘텐츠 SUBSCRIBE (빠름)
   → handleSessionSubscribe 호출
   → joinSession() → 세션 생성 (DB 저장)
   → Redis Pub/Sub으로 WatchingSessionChange 브로드캐스트
   → UI 업데이트 (내가 입장한 것 표시)

3. GET API 호출 (나중에)
   → 현재 시청자 목록 조회
   → UI에 전체 목록 표시 (초기 상태)
```
- 이후 다른 컨텐츠를 클릭하면 2번과 동일
# Cloud
# Content Batch
- **왜 배치를 사용했나?**
	- 대량 데이터 수집: 외부 API에서 수만 건의 콘텐츠를 주기적으로 수집
	- API Rate Limit 대응: 외부 API 제한을 고려한 순차 처리
	- 비동기 처리: 메인 서비스에 영향 없이 백그라운드 실행
	- 재시도/복구: Spring Batch의 재시도와 실패 처리 메커니즘 활용
	- 모니터링: Job 실행 상태와 성능 메트릭 수집
-  **배치 구조**
	- Job 구성
		- `dailyMovieUpdateJob`: 매일 다음날 개봉 예정 영화 수집
		- `dailyMovieUpdateJob`: 매일 다음날 방영 예정 TV 프로그램 수집
		- `dailySportsUpdateJob`: 매일 축구 경기 정보 수집
	- **Scheduler:** 매일 08:30에 깨어남.
		- 순차 실행: Movie → TV → Sports (각 Job 사이 2초 대기)
		- `@Scheduled(cron = "0 30 8 * * *", zone = "Asia/Seoul")`
	- **Reader:** 외부 API (TMDB/SportsDB) 호출.
	- **Processor:** 데이터 가공 (필요 없는 필드 버리기, 포맷 변경).
    - **Writer:** DB(PostgreSQL)와 검색엔진(OpenSearch)에 **동시에** 저장.
- 사용하는 외부 API
	- TMDB (The Movie Database)
		- 용도: 영화/드라마 메타데이터
		- 인증: Bearer Token
		- 엔드포인트: `/discover/movie`, `/discover/tv`
		- Rate Limit 대응: 페이지당 1초 대기, 10페이지마다 추가 대기
	- SportsDB
		- 용도: 축구 경기 정보
		- 인증: API Key (Query Parameter)
		- Rate Limit 대응: 날짜당 1초 대기
- 주요 구현 특징
	- OpenSearch 동기화
		- 배치로 수집한 콘텐츠를 PostgreSQL과 OpenSearch에 동시 저장
		- 검색 성능 향상
	- 메트릭 수집
		- Micrometer로 Job 실행 시간, 성공/실패 횟수, 수집된 콘텐츠 수 기록
		- 모니터링 대시보드 연동 가능
	- Rate Limit 대응
		- 페이지/날짜별 대기 시간 적용
		- 최대 페이지 제한 (500페이지)
# User Authentication (Spring Security) + 외부 API
- **전체 흐름 요약**
	- 로그인: 서버가 Access Token(30분) + Refresh Token(2주) 발급. Refresh Token은 Redis에 저장.
	- API 요청: 클라이언트는 Access Token만 사용.
	- Access Token 만료: API 요청 실패(401 Error).
	- 토큰 갱신: 클라이언트가 Refresh Token을 보냄.
	- 검증: 서버가 Redis에 있는 토큰과 비교.
		- 일치: 새 Access Token 발급.
		- 불일치/없음(로그아웃 상태): 재로그인 요청.
- **OAuth2 로그인 흐름 (핵심 로직 4단계)**
	1.  요청 (Request): 사용자가 '구글 로그인' 버튼을 누르면, 프론트엔드에서 구글 로그인 페이지로 리다이렉트합니다.
	2. 인증 및 동의 (Auth): 사용자가 구글에서 로그인을 완료하면, 구글이 우리 서버(백엔드)로 **인증 코드**(Code)를 보내줍니다.
	3. 정보 획득 (Exchange & UserInfo):
		- Spring Security 내부(`OAuth2LoginAuthenticationFilter`)가 이 '코드'를 가지고 구글 서버에 다시 요청해 **구글 액세스 토큰**을 받아옵니다.
		- **구글 액세스 토큰**으로 구글의 사용자 정보 API를 호출해서 이메일, 이름, 프로필 사진 등을 가져옵니다. (이 과정을 `DefaultOAuth2UserService가` 담당합니다.)
	4. 우리 앱 로그인 (DB & JWT):
		- 받아온 이메일이 우리 DB(users 테이블)에 있는지 확인합니다.
			- 있으면: 바로 로그인 처리.
			- 없으면: users 테이블에 이메일과 소셜 정보를 저장(회원가입) 후 로그인 처리.
		- 최종적으로 클라이언트에게는 **우리 서비스 전용 JWT를 발급해줍니다**.
- **회원가입 (데이터 정합성과 성능)**
	- '일반 가입'과 '소셜 가입' 두 가지 입구가 있지만, 결국 내부적으로는 하나의 `User` 테이블에 저장됨
	- **BCrypt:** 비밀번호를 DB에 그대로 저장하면 보안 사고 시 다 털려서 단방향 해시 함수인 BCrypt로 암호화해서 저장 (복호화 불가능, 비교만 가능)
	- **Caffeine Cache (`@CachePut`):** 회원가입 직후 해당 유저 정보를 로컬 캐시(Caffeine)에 넣음(@CachePut). 가입 직후 바로 로그인을 하거나 조회를 할 때 DB를 거치지 않고 빠르게 응답하기 위함
- **Spring Security (JWT + Redis 전략)**
	- **JWT - Stateless (무상태)** 서버가 클라이언트의 상태(세션)를 기억하지 않음
		- 요청이 올 때마다 토큰을 검사해서 누군지 판단함
		- 서버 확장(Scale-out)에 유리함.
	- **Access/Refresh Token 분리**
	    - **Access Token:** 유효기간이 짧음(30분). 탈취당해도 금방 만료되게 하여 보안성을 높임. 매 요청마다 헤더에 담김.
	    - **Refresh Token:** 유효기간이 김(24시간). **Redis에 저장.** Access Token이 만료되면 이걸로 재발급받음.
	    - **왜 Redis인가?**
		    - **속도:** Refresh Token 검증은 꽤 빈번하게 일어납니다. Disk 기반인 MySQL보다 **In-memory(RAM)** 기반인 Redis가 훨씬 빠릅니다.
		    - **TTL (자동 삭제):** MySQL은 만료된 토큰을 지우려면 별도의 스케줄러(Batch)를 돌려야 하지만, Redis는 만료 시간이 지나면 알아서 데이터를 삭제해 주므로 관리가 효율적입니다.
- **외부 API (동기 vs 비동기)**
	- **WebClient**
		- 예전의 `RestTemplate`은 요청을 보내고 응답이 올 때까지 스레드가 멈춥니다(Blocking). 
		- `WebClient`는 응답을 기다리는 동안 스레드가 다른 일을 할 수 있습니다(Non-blocking). 외부 API 응답이 늦어도 내 서버 전체가 느려지지 않게 하기 위함입니다.

# Kafka
- 카프카는 어디서 사용하였나요?
	- Kafka는 비동기 이벤트 처리를 위해 사용했습니다.
	- 제가 맡은 부분은 아니지만, Spring의 @TransactionalEventListener를 사용해 DB 트랜잭션이 커밋된 후에만 이벤트를 Kafka로 발행하도록 했습니다. 이렇게 하면 DB 저장이 확정된 후에만 이벤트가 발행되어 데이터 일관성을 보장할 수 있습니다.
	- 발행되는 이벤트는 알림 생성, DM 생성, 플레이리스트 생성, 메일 발송 등이 있고, Consumer에서는 @KafkaListener로 구독해 처리합니다. 
	- 예를 들어, 사용자가 플레이리스트를 생성하면 DB에 저장된 후 Kafka로 이벤트가 발행되고, Consumer에서 이를 구독해 팔로워들에게 알림을 보내는 방식입니다. 이렇게 하면 플레이리스트 생성 API의 응답 시간에 영향을 주지 않으면서도 비동기로 알림을 처리할 수 있습니다.
- 카프카는 왜 사용하셨나요?
	- 네, 카프카를 사용하지 않으면 동기로 처리해야해서 어플리케이션이 느려질 수 있습니다. 
	- 예를 들면 따로 분리하지 않으면, 회원가입을 할때 메일이 발송해야지만 회원가입이 완료가 되지만, 따로 분리를 하면 회원가입 후 바로 회원가입 완료를 띄우고 메일 발송은 따로 카프카가 처리해주면 됩니다.
	- 또, 확장성을 고려했습니다. 
	- 다중 서버 환경에서 특정 유저가 어떤 서버에 연결되어 있는지 알 수 없기 때문에, 카프카를 메시지 브로커로 사용하여 이벤트를 전파하고, 각 서버의 Consumer가 이를 받아 실시간 알림을 푸시하도록 구현되어 있습니다.

# Playlist
- 주요 기능
	1. CRUD: 생성, 조회, 수정, 삭제
	2. 구독: 구독/구독 취소, 구독자 수 관리 (subscriberCount)
	3. 콘텐츠 관리: 플레이리스트에 콘텐츠 추가/삭제
	4. 이벤트: 플레이리스트 생성 시 Kafka 이벤트 발행
	5. 알림: 콘텐츠 추가 시 구독자에게 알림
# DM
- 주요 기능
	1. 채팅방(Conversation): 1:1 대화방 생성, 중복 방지
	2. 실시간 메시지: WebSocket(STOMP)로 실시간 전송
	3. 읽음 처리: 메시지 읽음 상태 관리, 채팅방 hasUnread 플래그
	4. 이벤트: DM 생성 시 Kafka 이벤트 발행해서 받는 유저에게 DM 왔다는 알림 보냄
	5. 권한 검증: 채팅방 참여자만 접근 가능