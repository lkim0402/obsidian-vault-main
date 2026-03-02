# Google Cloud에서 모델 학습부터 서빙까지

# MLOps
- AWS - 기계 학습(ML)워크플로우, 배포를 자동화하고 단순화하는 일련의 과행
- 머신러닝 모델의 개발부터 배포, 운영, 유지보수 까지 모든 과정을 효율적으로 관리하기 위한 방식
- AI 어플리케이션을 만드는 회사는 많지만 실제로 배포한 회사는 적음
- 모델 개발은 재미있는 부분일 뿐
	![[Pasted image 20250604103358.png]]
	- 5% 밖에 안됨
	- 꾸준히 데이터를 모니터링하야함
- 머신러닝 모델 운영의 어려움
	- 복잡한 인프라 관리
		- 컴퓨팅 자원, 스토리지, 네트워크 등을 다루기 어려움
	- 높은 운영 비용과 확장성
		- 며칠씩 걸리는 학습 시간, 예측 불가능한 트래픽 등
	- 어려운 위크플로우 통함
		- 데이터 수집부터 전처리, 모델 학습, 평가, 배포 end to end 솔루션으로 통합 어려움
	- 보안과 지속적인 관리 필요
- ML Lifecycle
	![[Pasted image 20250604103634.png]]
	- ml의 모든 과정을 자동화하는 파피프라인
	- advantages
		- Reusability - 재사용성
			- 새로운 데이터를 받을 때 코드를 크게 바꾸지 않아도 됨
		- 자동화 규칙 rules around automation	
# MLOps by Vertex AI
![[Pasted image 20250604104751.png]]
- 인프라를 직접 구축하는 대신 솔루션에 집중
- 신속하게 production workflow로 전환 가능
- 처음부터 끝까지 (end to end), 혹은 1개만 사용 가능

#### 데이터 추출 전처리
- Vertex Ai 관리형 데이터 세트
	- 데이터셋을 통합 관리
	- 모든 팀이 유관된 데이터를 사용할 수 있게 함
	- (학습, 테스트, 검증)
- Vertex Ai Feature Store (optional)
	- 머신러닝 피쳐 공유, 재사용
		- 쉬운 검색, 추출, 권한 권리
	- 빠른 고성능 머신러닝 피처 서빙
	- Training-Service Skew 해소
		- 피쳐값을 1번만 계산
#### 모델 학습
- Vertex Ai Workbench
	- jupyter Notebook으로 제공됨
	- 라이브러리, framework게 설치되어있음
	- 깃허브와 동기화
	- GPU가속기 지원
- Vertex Ai Custom training
	- 코드를 짜서 모델을 트레이닝할 수 있는 환경
	- 완전 관리형 인프라 (모델 개발에 집중)
	- 다양한 ml framework 지원 + python sdk
		- tensorflow, pytorch, etc
	- 고성능 학습 환경
		- Cloud profiler - 디버깅을 쉽게 가능
	- Reduction Server
- AutoML
	- 코딩 없이 데잍를 통해 최적의 ML을 자독으로 학습하고 배포하는 ML 자동화 플랫폼!
	- 머신러닝 초보자가 쉽게 접근 가능
#### 모델 평가
- Vertex AI Model Registry
	- 모델 버전 관리
	- 모델의 성능 핵심 평가 지표 + 시각화
	- 다양한 데이터 슬라이스 및 평가된 어노테이션별 모델 성능 분석
#### 예측 생성
- Vertex Ai Prediction
	- 모델 서빙에 관한 인프라
	- 다른 서비스들과 통합 가능
	- batch prediction / online prediction (nest api)
#### 지속적인 모니터링
- Vertex Ai model monitoring
	- 배포된 모델의 성능 저하를 지속적으로 감지하고 경고하는 솔루션
### 자동화
- Vertex AI Pipeline
	- ML워크플로우 구성 단계를 자동화 (위의 것들) + 오케스트레이션하는 MLOps 파이프라인 관리
	- kuberflow pipeline 
	- 서버리스 (사용한 만큼만)
#### Adding Gen AI
- Gemini - 멀티모달 생성형 AI
	- 위 과정 중 gemini랑 같이 사용 가능
	- Agent 만들어서 합치는 것도 가능함
# Customer Cases