
- Data engineer
- 데이터를 활용하는 사람들을 위해 데이터를 저장, 가공, 처리하는 직군
- 배경
	- 앱/웹 서비스: Database에 데이터가 저장됨 (MySQL, PostgreSQL, MariaDB)
	- 서비스를 위한 데이터를 저장
- Database에 저장된 데이터를 Data Warehouse로 옮기는 일
	- Data Warehouse
		- 데이터 분석에 특화된 데이터베이스
		- GCP의 BigQuery, AWS의 Redshift, Snowflake
- ETL pipeline
	- Extract
		- 데이터 추출
		- 서비스의 database, 앱/웹의 로그 데이터를 추출
	- Transform
		- 데이터를 잘 활용할 수 있도록 변환
	- load
		- 변환된 데이터를 사용할 수 있도록 설정하고 불러오

- Is data labeled consistently?
	- Ex (speech recognition): How much silence before/after each clip? Remove noise? Volume normalization?