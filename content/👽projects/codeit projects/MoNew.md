# 뉴스 배치
- 뉴스 배치의 전체적인 흐름을 설명해주세요.
	- 뉴스 배치는 매 시간 정각에 실행되며, DB에 저장된 모든 사용자 관심사 키워드를 조회하는 것으로 시작합니다.
	- 수집 방식은 두 가지입니다. 
		- 첫째, **네이버 오픈 API**를 호출하여 특정 키워드에 대한 뉴스 검색 결과를 JSON 형태로 받아옵니다. 
		- 둘째, **주요 언론사(한경, 조선, 연합)의 RSS 피드**를 크롤링합니다. RSS는 전체 기사를 받아온 뒤, 서버 메모리 상에서 해당 키워드가 제목이나 본문에 포함된 기사만 필터링하는 방식으로 수집합니다.
	- 이렇게 수집된 데이터는 중복을 제거한 뒤 DB에 저장됩니다.
## 실행 순서
1. **스케줄러 실행 (`NewsBatchScheduler`)**
    - 매 시간 정각(`0 0 * * * *`)에 `runNewsCollectionJob()` 메서드가 실행됩니다.
    - 이 메서드는 `NewsCollectJob`이라는 배치 작업을 시작시킵니다.
2. **배치 작업 시작 (`BatchConfig`)**
    - **읽기 (`interestReader`):** DB에 저장된 모든 관심사(Interest)의 ID(UUID)를 전부 가져옵니다. (예: 사용자들이 등록한 "삼성전자", "부동산", "AI" 등의 관심사 ID 목록)
    - **처리 (`interestProcessor`):** 별다른 가공 없이 ID를 그대로 넘깁니다.
    - **쓰기 (`itemWriter`):** 가져온 ID들을 10개씩 묶어서(Chunk) 처리합니다. 각 ID마다 `articleService.saveByInterest(ID)`를 호출합니다.
3. **실제 수집 (`Collector` 구현체들)**
    - `articleService` 내부(코드는 없지만 흐름상)에서 해당 ID에 맞는 키워드(예: "AI") 꺼내고, 아래 두 가지 수집기(`NaverApiCollector`, `RssCollector`)에게 그 키워드를 던져줍니다.
## 실제 수집
-  네이버 검색 API (`NaverApiCollector`)
	- **출처:** 네이버 뉴스 검색 서버 (`openapi.naver.com/v1/search/news.json`)
	- **방식:**
	    1. `WebClient`를 사용해 네이버 서버에 HTTP GET 요청을 보냅니다.
	    2. 이때 쿼리 파라미터(`?query=키워드`)에 **수집하려는 키워드**를 실어서 보냅니다.
	    3. 네이버가 찾아준 뉴스 결과를 JSON으로 받아서 자바 객체(`ArticleDto`)로 변환합니다.
- 언론사 RSS 피드 (`RssCollector`)
	- **출처:** 코드 내 `rssMap` 변수에 하드코딩된 3개 사이트 주소입니다.
	    1. 한국경제 (`hankyung.com/feed/all-news`)
	    2. 조선일보 (`chosun.com/...`)
	    3. 연합뉴스TV (`yonhapnewstv.co.kr/...`)
	- **방식:**
	    1. `Jsoup` 라이브러리를 사용해 위 3개 URL에 접속하여 XML 문서를 통째로 긁어옵니다.
	    2. 가져온 뉴스 목록 전체에서, 제목이나 요약 내용에 **우리가 찾는 키워드**가 포함되어 있는지 자바 코드(`stream().filter(...)`)로 검사합니다.
	    3. 키워드가 포함된 뉴스만 남기고 나머지는 버립니다.