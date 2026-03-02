- courses i will watch & learn before the 23rd
	- [x] [[LangChain]] review - [Functions, Tools and Agents with LangChain](https://learn.deeplearning.ai/courses/functions-tools-agents-langchain/lesson/rtwb1/introduction) 
	- [x] langgraph documentation 훑기..
- stuff to study/do
	- [x] finda 도메인 지식/회사 상품 좀 알아보기
	- [x] 개발자 인턴 준비 - 노션 템플릿
- [갓스물 인턴은 핀테크 스타트업에서 무슨 일을 할까?👨‍💻](https://sungcho.substack.com/p/6ab)
- tech stack
	- Core Tech: Python, SQL, OpenAI API(GPT-4o), LangChain/LangGraph, Vector DB
	- Automation & Ops: n8n, Slack Workflow API, Airflow 
	- Collaboration: Slack, Jira, Confluence, GitHub
- [[massive list of things to learn]]
# to remember
- 인사 환하게 하기!! (기본 중에 기본)
- 수첩 + 볼펜 -> 무조건 메모
	- 사내규칙, 업무
	- 사수가 알려주는 내용 최대한 받으려고 노력 (한번에 알아듣기, 똑같은 말 여러번 반복하는거 좋아하는 사람 없음)
- 지각 절대 하지 않기
- documentation
	- 아주 작은 것이라도 구체적으로 기록하고 숫자, 증거로 남기는 것이 중요
	- 업무일지 - todo list 작성, 어떻게 일했는지 단순 업무도 성과로 변환시키려는 노력 필요
# small research
## 금융 도메인
- **대출** 
	- 은행/금융기관이 개인이나 기업에게 돈을 빌려주는 것
	- *대출 한도* = 내가 빌릴 수있는 최대 금액. 은행마다 평가 기준이 다름. 신용,소득,자산,기존대출 등에 따라 결정됨
	- *금리* = 은행 등 금융기관에서 돈을 빌릴 때, 원금에 대해 지불해야 하는 이자의 비율(가격)
		- 대출 금리 = loan interest rate
- **DSR**
	- debt service ratio, 총부채원리금상환비율
	- 연 소득 대비 갚아야 할 원금과 이자의 비율 (소득에 비래 대출이 얼마나 있는지)
		- 1년간 갚을 대출 원리금(원금+이자) ÷ 연소득 x 100
	- *이 비율이 높을수록 대출이 많다고 판단되어, 추가 대출이 어려울 수 있음*
- **finda** -> 대출 비교 플랫폼
	- 메인
		- 1 분만에 대출 비교부터 신청 (국내 최다 94개 금융사와 400여개, 상품비교로 한도는 올리고 금리는 낮추기)
		- 이미 있다면, 가진 대출보다 더 나은 조건 갈아타기
			- 금리는 내리고 한도는 올리고
		- AI로 정확하게 -> 나와 딱 맞는 내 대출 조건
			- 나와 비슷한 조건의 사람들의 금리와 한도 조건을 기반으로 대출 조건을 예측
	- 자산관리
		- 계좌와 카드를 연결하고 쓸 수있는 현금부터 나갈 돈까지 관리 + 금융 습관 만들기
## AI (research)
- [AI 시대, 속도와 안전을 동시에 설계하는 CISO](https://www.post.finda.co.kr/people260120)
	- hands on leadership => "핀다를 보면서 느낀 건, “여기는 형식적인 보안이 아니라, **실제 문제를 풀어야만 살아남는 곳**이겠구나.”였습니다."
	- 두가지
		- 데이터 기반 정보보안 거버넌스
		- AI를 보안팀의 힘을 키우는 기술 (위협 분석, 보안 코드 리뷰, 정책 준수 여부 검증)
	- AI를 쓰는 조직의 보안 설계
		- 모든 AI사용이 반드시 한 관문 (*AI Gateway*)를 통과하도록 -> 모든 서비스나 시스템에서 생성형 AI를 호툴할 때, 직접 위부 API를 두드리는 대신 게이트웨이를 경유하게 만드는 구조
			- 게이트웨이에서 어떤 모델을 쓰는지 / 어느 리전에 데이터가 전달되는지 / 입력 값에 민감정보가 섞여 있는지 / 출력 값에 위험한 정보가 포함되는지를 중앙에서 통제할 수 있음
			- 민감정보는 자동으로 마스킹하거나 차단하고, 개인(신용)정보가 포함된 요청은 **국내 리전 모델만 쓰게 강제**할 수 있음
	- 생성형 AI를 쓰면서 조심해야할것
		- *선의의 직원이 만든 정보 유출*
			- 개발자가 코드를 챗봇에 고객식별정보나 인증토큰 등이 섞여있음
			- 회의록을 요약하려고 회사 전략 문서나 제휴 계약 내용이 포함된 파일 그래도 업로드
	- 성장하는 엔지니어 growing engineer
		- 주어진 보안 체크리스트를 그냥 수행하는 사람이 아니라, 이건 . 왜이렇게 되어 있을까, 더 나은 구조는 없을까를 직접 고민
		- 기존 기술에 머무르지 않고 ai, cloud, security등 새로운 기술을 계속 탐구
	


# 1 month evaluation, logs
- [[intern log template]]
## Week 1 (2.23 - 2.27)
- [[2.23.26 - onboarding]]
- [[2.24.26 - 2nd day]]
- [[2.25.26 - 3rd day]]
- [[2.26.26 -4th day]]
- [[2.27.26 - 5th day]]
	- [[해모님 커피챗 (2.27.26)]]
## Week 2 (3.3-3.6)
