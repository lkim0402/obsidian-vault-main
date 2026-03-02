>[!Purpose]
>Logs alone have no value unless used to quickly identify causes when issues arise.
>Log searching is needed in situations such as:
>- Diagnosing causes of system failures
>- Tracing logs related to user complaints
>- Identifying bottlenecks causing slow responses
>- Checking errors from external APIs
>- Determining reproduction conditions for unexpected exceptions

# Basic log searching methods
```shell
# 키워드 기반 검색
$ grep "ERROR" app.log
$ grep "[USER_CREATE]" app.log

# 시간 범위 지정 (로그가 타임스탬프 포함 시)
$ awk '/2024-12-01 10:00/,/2024-12-01 11:00/' app.log

# 특정 사용자 ID 로그 찾기
$ grep "userId=123" app.log
```

Using regex
```shell
# 예: userId가 숫자인 로그만 추출
$ grep -E "userId=[0-9]+" app.log

# 예: 특정 모듈에서 발생한 WARN 이상 로그 찾기
$ grep -E "\\[(USER|ORDER)_.*\\]" app.log | grep -E "WARN|ERROR"
```

# Analysis w/ Log Format
Example log:
```
2025-07-25 10:32:01.345 [http-nio-8080-exec-1] INFO  c.e.UserService - [USER_CREATE] 사용자 저장 완료: id=15 username=maru
```

- Analyzing logs based on this format helps efficiently filter and understand events.
	- **Timestamp:** `2025-07-25 10:32:01.345` — When the event occurred
	- **Thread:** `[http-nio-8080-exec-1]` — The thread that handled the request
	- **Log level:** `INFO` — Severity of the event
	- **Logger:** `c.e.UserService` — Source class or package
	- **Message:** `[USER_CREATE] 사용자 저장 완료: id=15 username=maru` — Actual event details, with a searchable tag `[USER_CREATE]` and contextual data (`id`, `username`)

# # Using Logs for Problem Solving

#### Example 1: Tracing Cause of Slow Response
```
[ORDER_PROCESS] 주문 처리 시작: orderId=11
... (2초 경과)
[ORDER_PROCESS] DB 저장 완료: orderId=11
... (3초 경과)
[ORDER_PROCESS] 외부 API 호출 실패: timeout
```
- Comparing timestamps at each step reveals where delays occur.
- Quickly identify that the issue is caused by an API timeout.
#### Example 2: Tracking User Complaints
```java
$ grep "username=honggildong" app.log
```
- Enables tracking when the user logged in, made requests, or experienced failures.
#### Example 3: Determining Exception Reproduction Conditions
```
[USER_DELETE] 사용자 삭제 실패: userId=55, status=INACTIVE
```
- Indicates a restriction on deleting users with `status=INACTIVE`.
- Allows review of condition handling in both frontend and backend.
