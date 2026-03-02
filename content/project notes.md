# 사람인에서 공고 리스트 가져오기
- 오피셜한 API가 아니라 사람인의 내부 API 
	- `https://api-hiring.saramin.co.kr/api/recruits`를 부름 
- `page.request.get`
	- `page.request` is special, it uses Playwright's browser context which includes all the cookies and session tokesn that were set when u logged in earlier in the script -> saramin thinks it's just the normal website making that API call
- 요약
	1. You logged in via browser automation (Playwright)
	2. Saramin set auth cookies in the browser session
	3. `page.request.get()` piggybacks on those cookies
	4. Saramin's backend sees a valid authenticated request and returns the data
```python
async def fetch_unread_recruits(page) -> list:
    base_url = "https://api-hiring.saramin.co.kr/api/recruits"
    unread_recruits = []
    page_num = 0
    page_size = 20

    while True:
        response = await page.request.get(
            base_url,
            params={
                "searchKey": "title",
                "searchValue": "",
                "platform": "all",
                "recruitType": "all",
                "recruitMode": "ing",
                "sortType": "editDt",
                "size": page_size,
                "page": page_num
            }
        )
        data = await response.json()

        if not data.get("success"):
            print(f"API 요청 실패: {data}")
            break

        content = data["result"]["content"]
        if not content:
            break  # 더 이상 데이터 없음

        for recruit in content:
            # statusValue > 0 이면 미열람 지원자 있음
            if recruit.get("closeApplyCnt", 0) > 0:
                unread_recruits.append(recruit)

        # 마지막 페이지 확인
        total_elements = data["result"].get("totalElements", 0)
        fetched_so_far = (page_num + 1) * page_size
        print(f"API 페이지 {page_num}: {len(content)}개 공고 확인 (총 {total_elements}개)")
        if fetched_so_far >= total_elements:
            break
        page_num += 1

    return unread_recruits
```

# 파일 저장되는 로직
- `job_postings/<공고명>/<지원자명>/파일명`
# 파일 다운 뒤 greeting api로 그리팅 업로드
- 플로우
	1. 업로드 URL 생성 
		- `POST /openapi/file-upload`
			- body `{ "openingId": 123, "fileName": "김이준_이력서.pdf" }`
		- 리턴값: `{ "fileToken": "...", "uploadUrl": "https://..." }`
	2. 업로드 실행
		- `PUT <uploadUrl>` with the raw file bytes
		- 리턴값: 
	3. 유저 등록
		- 2에서
		- `POST /openapi/applicant`
```json
{
  "openingId": 123,
  "name": "김이준",
  "documents": [{ "fileToken": "...", "docName": "이력서" }],
  "questionnaires": []
}
```

- 코드 로직
	- 각 공고별 for loop하면서 각 공고의 지원자 이름을 꺼냄
	- 각 지원자별:
		- 그리팅으로 **지원자들의 파일을 업로드**하고 각 파일의 토큰값을 받아옴
			- `POST https://oapi.greetinghr.com/openapi/file-upload`
		- 각 파일의 토큰값을 받아온 뒤 document list를 만들고, 지원자 정보를 조합해서 지원자 등록
			- `POST https://oapi.greetinghr.com/openapi/applicant`

```python
for recruit in unread_recruits:            
	rec_idx = recruit["recIdx"]
	recruit_name = recruit["title"]
	print(f"공고 이동: '{recruit_name}' (recIdx={rec_idx})")
	folder_path = ensure_job_posting_folder(recruit_name)
	
	# ... 그 외 로직

	# 그리팅에 지원자 등록
	opening_id = job_map.get(recruit_name)
	if opening_id:
		for name in os.listdir(folder_path):
			applicant_folder = os.path.join(folder_path, name)
			if os.path.isdir(applicant_folder):
				greeting_upload.register_applicant_to_greeting(
					opening_id=opening_id,
					name=name,
					applicant_folder=applicant_folder,
				)
	else:
		print(f"⚠️ '{recruit_name}'에 대한 그리팅 공고를 찾지 못해 등록 건너뜀")
```

```python
def register_applicant_to_greeting(
    opening_id: int,
    name: str,
    applicant_folder: str,
    phone: Optional[str] = None,
    referer: str = "사람인",
) -> dict:
    documents = []

    for filename in sorted(os.listdir(applicant_folder)):
        file_path = os.path.join(applicant_folder, filename)
        if not os.path.isfile(file_path):
            continue

        print(f"파일 업로드 중: {filename}")
        token = upload_file_to_greeting(opening_id, file_path)
        documents.append({
            "fileToken": token,
            "fileUrl": None,
            "fileName": None,
            "docName": infer_doc_name(filename),
        })

    if not documents:
        raise ValueError(f"업로드할 파일이 없습니다: {applicant_folder}")

    resp = requests.post(
        f"{BASE_URL}/applicant",
        headers = _headers(),
        json = { # payload
            "openingId": opening_id,
            "name": name,
            "phone": phone.replace("-", "") if phone else None,
            "referer": referer,
            "documents": documents,
            "questionnaires": [],
        },
    )
    resp.raise_for_status()
    print(f"그리팅 지원자 등록 완료: {name}")
    return resp.json()
```
## greeting 공고 이름, 사람인 공고 이름 확인
- 현재는 common substring찾아서, 이게 가장 길게 겹치는 애들을 dictionary에 매핑시킴
