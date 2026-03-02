# java tips
- 특별한 경우 아니면 Object 쓰지 마자
	- 위험함
- to review stuff
	- class VS object <<<<<<<<<<<< 복습하기
	- jcf 자료구조 간단히 블로그에 정리하기
	- Optional in Stream
- java, springboot 버젼 정확히 알기<<<<
- 자바 깊게 공부할 때, lambda, stream, generic
	- 이게 진입점임 ㅋ
	- 얘녜도 못하면 걍 입구에 서있는거임.
- 마크 mod -> java lmaoooo
	- 단어 뜻 외워두면 편함
- 객체지향은 polymorphism, abstraction 필수임

- 우리는 spring mvc + spring security + spring data jpa

- 공식문서 spring,java는 불편하게 되어있지만 익숙해져야함<<<<<<<<<<<!!!!!!!
	- 오히려 인공지능의 시대에서 공신문서를 잘 읽을 수 있는지 없는지가 좋은 개발자가 될수있을지 판별함

- spring VS springboot 차이 알아야함
	- springboot
		- bootstrap, 설정을 다 해줌, 
	- boot랑 spring 버젼이 다름

- var
	- 어떠한 객체가 들어갈수있게 추럼
	- 일반적인 경우에는 사용x
	- 테스트할떄는 편리함
	- **kotlin에서는 사용!**
		- kotlin + spring = 코프링
# backend/개발 tips

- Web server VS WAS (web application server)
	- **차이점 무조건 알아야함!!!!!!!!!!!**
	- WAS는 전통적인 서버임
		- spring 은 WAS <<<<<<<<
	- Webserver는 정적인 데이터임

- Postgres port number - 5432
- total number of available network ports on a computer
	- 포트번호 FFFF 256 * 256  = 65536 -> 0 - 65535 포트 실제로 쓸 수 있는데 잘 안씀


- **DB, 트랜색션, 동시성, 데드락**
	- 무조건............... 이거 모르면 kafka, rabbitq 못씀

- crud 에서 젤 중요한거
	- READ
		- 이거 위주로 생각을 하는 마인드셋
		- db를 다루는 것도 이거임

- memory overhead?

- backend -> devops/cloud
	- go lang 배워놓으면 좋음! (kotlin보다)
- 습관화
	- 코드를 쓸 때 ctrl click해서 더 읽어보는 습관
- sql는 가장 고주순 언어임
- supersets
	- java -> kotlin
	- js -> ts
	- c -> rust

- 성능 계산
	- FE - 유입이 얼마나 더 잘되는지
	- BE - 비용을 얼마나 절감하는지 (성능을 계선해서)
		- **수치화 해야함!!!**

- career path을 미리밀 그리자
	- 미리미리 그리는 사람들이 남들보다 좋은 회사에 간다
	- 대회활동, summit 많이 하자
- 기본적으로 **DevOps 알아야함**
	- 근데 이 직군은 그렇게 많진 않은데, 그냥 백엔드가 하는 경우가 많음

- 바이블
	- clean code
	- clean architecture
	- 최범균의 jsp?
		- 그리고 spring을 하면 이해도가 10배 늘어날거임
	- 나중되면 - JPA 프로그래밍
- **Session VS Token VS Cookies**
	- session
		- 사용자의 정보를 저장하는 공간, 서버에서 관리
		- 로그인 -> 사용자정보는 session이라는 곳에 보관 (서버에서 보관)
			- 서버에 있기 때문에 안전 (외부에 유출 x)
		- 상태를 끝날 때 그때 지워짐 (ex. 로그아웃)
	- session vs token
		- session - in server
		- **이거 면접때 나올 수 있음 (session vs token, 왜 토큰대신 사용?)**
	- cookie
		- 3rd party cookie - 검색내역을 쿠키에 저장해서 다른 곳으로 보냄 (내 정보 유출)
# interview
- faang은 그냥 레퍼럴 받아야함 < ㅋㅋ
	- 가독성 좋은 코드 
		- 짧은 코드 x
		- 분석하는 시간이 적음 o
			- 변수명이 길더라도 분석이 적게 된다면 좋은 코드임
			- 변수명 중요함
		- 코드 리뷰 회사에서 많이 함
			- 요즘은 문서화가 잘 되어있음
- 생각보다 
	- 변수 범위를 면접에서 물어볼 수 있음 (boundary) -> int and long
	- 제대로 공부했다는 느낌
- overloading vs override 차이 면접에 물어볼 수 있음
- 면접 단골
	- garbage collection
- SOLID 나올 수 있음


# CS

- 나중에 시간날때
	- research why quantum computers are fast!
	- poker


- **REST API 무조건 알아야함....개발자가 모르면 말이 안됨 > 면접 단골질문, 별 10000개**
	- spring은 이것도 잘 지원해줌
- 결국 네트워킹 알아야함 - **네트워크, DB는 기본적인 CS 지식임. 필수임!!**
	- http
		- 무상태성
		- 비연결성<<
			- 내가 요청을 보내고 응답이 오던말든 끊김
			- 한번 보내면 끝 - 연결이 지속되지 않음
		- 실시간 연결을 위한 기술 - socket
	- HTTP -> TCP/IP 알아야함 꼭 <<<
	- **면접 단골: TCP, UDP 차이 (개발자라면 걍 알아야함), 나중에 정리 꼭 하기**
		- TCP: 신뢰할 수 있는 통신
		- UDP더 빠름
			- 그냥 상대방에게 던짐
	- 내 컴터의 IP 추소
		- 127.0.0.1 = localhost
		- `sudo lsof -i :8080` 
	- DNS
- **SSR, CSR**
	- CSR - client side rendering
		- spring mvc 기반 rest api
	- SSR
		- 웹 페이지 서버 개발
		- 내부에서 html을 만들어서 보냄

- system architecture
	- martin folwer - check his books
- 실습과정 디버깅 실습
	- 디버깅 실력은 내 개발자 인생을 결정함
# study
**질문, 답변, 꾸준히 기록하기**
- 기록은 취업준비, 취업할때 되돌아볼수있음
- 미래의 나의 자산이 됨


- 취업준비할때
	- 생각보다 스토리텔링이 중요 -> 왜 이걸 좋아하는지 -> 근거자료
		- 유튭에서 우연히 정렬 알고리즘 시각화를 봤는데 흥미로웠다
	- 어떠한 얘기로 여기 들어왔고 이러한 것들은 공감포인트가 될 수 있음
		- 블로그가 젤 좋긴 함
		- 서류는 안보더라도 이런것들음 봄...
			- github
			- commit내용 엉망X