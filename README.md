# NIE-for-Improving-Children-s-Reading-Comprehension-Skills-
어린이 문해력 향상을 위한 신문활용교육 서비스

### 1. Data Crawling and Pre-processing
- 신문 텍스트 데이터 수집
  - 최근 10개년(2013년~2022년) 국내 4대 언론사(중앙일보, 경향신문, 동아일보, 한겨레)의 신문 기사 텍스트를 수집.
  - 빅카인즈 API 활용하여 신문활용교육에 적합한 내용을 담고 있는 카테고리 4종(정치, 사회, 경제, 국제)에 대하여, 당대에 가장 관심이 많았던 기사를 수집
- 텍스트 전처리
  - 특수기호, 한자, 작성자 이메일 등을 제거하는 전처리 작업을 수행
 
### 2. Create Variables
- 변수 생성을 위한 추가 데이터 수집
  - ‘신문기사 출현 빈도’를 측정하기 위해 ‘물결21’ 말뭉치의 형태소별 빈도수 324,033건을 수집
  - ‘일상 출현 빈도’를 측정하기 위해 국립국어원 ‘모두의 말뭉치’ 중 온라인 게시자료 말뭉치의 형태소별 빈도수 426,636건과, 「현대국어 사용빈도(국립국어원)」의 형태소별 빈도수 58,433건을 수집
  - ‘기초 어휘 난이도’를 측정하기 위해 국립국어원의 「국어 기초 어휘 선정 및 어휘 등급화 연구」의 어휘 10,577건을 정제하여 수준별 어휘목록을 수집
- 신문 텍스트에 대한 정량적인 판단을 위한 변수 생성
    - 일상 출현 빈도 : 국립국어원 모두의 말뭉치를 활용
    - 신문 기사 출현 빈도 : 물결21 말뭉치를 활용
    - 기초 어휘 난이도 : 국립국어원 「국어 기초 어휘 선정 및 어휘 등급화 연구」 활용
    - 평균 문장 길이 : 바른 형태소 분석기를 활용한 문장 개수
    - 안은문장 비율 : 바른 형태소 분석기를 활용하여 전성어미 개수 파악 후 (전성어미 개수) $\div$ (기사 내 총 문장 개수)로 계산
    - 이어진문장 비율 : 바른 형태소 분석기를 활용하여 기사 내 총 연결어미 개수를 파악하고 (기사 내 총 연결어미 개수) $\div$ (기사 내 총 문장 개수’로 계산)로 계산
    - NF-iDF (어려운 신문어휘 점수) : 일상에서는 잘 쓰이지 않는 어휘이면서, 신문에서 많이 쓰이는 어휘는 어렵다고 판단하여, (신문 기사 출현 빈도) $\div$ (일상 출현 빈도)로 계산
- 변수를 생성하는 일련의 과정에서는 [바른 형태소 분석기](https://bareun.ai/)를 사용

### 3. Factor Analysis
- 요인 분석
  - 수집한 변수를 바탕으로 신문 기사의 난이도를 측정하기 위해 요인분석 수행
  - 설명 분산 비율과 신뢰도를 바탕으로 변수를 선택
  - 직교회전, 사각회전 등 다양한 방법을 수행
  - 최종적으로 전체 데이터셋의 약 72%를 설명할 수 있는 내재요인 3가지를 추출하는 데 성공
  - 내재요인을 바탕으로 신문기사의 난이도를 측정하는 **이독성(Readability)** 지표를 생성
    
  ![image](https://github.com/Sangvierr/NIE-for-Improving-Children-s-Reading-Comprehension-Skills-/assets/165464507/a951b145-5a85-43e3-9928-70a7e3a6c90e)

### 4. NewsletterGenerator
- 뉴스레터 작성 모델 개발
  - 생성형 AI의 대표적인 모델인 GPT를 활용하여 일반기사를 아이들이 읽기 쉬운 뉴스레터 형태로 작성하는 모델 개발
  - Chain-of-Thought Prompting Elicits Reasoning in Large Language Models(Wei et al.)을 참고하여 GPT 모델이 논리적인 순서에 따라 뉴스레터를 작성할 수 있도록 Prompt를 작성
  - 2023년 8월부터 GPT-3.5-Turbo 모델에 Fine-tuning이 가능해짐. 따라서 <뉴닉> 등의 뉴스레터 등을 참고하여 데이터셋을 직접 생성 후 GPT 모델에게 학습시킴
  - 학습된 모델(Fine-tuned GPT)을 BigKids-GPT 모델로 사용해 일관적으로 뉴스레터를 작성함
- 내용일치 퀴즈 모델 개발
  - GPT-3.5-Turbo 모델과 GPT-4-Turbo 모델을 활용하여 퀴즈 생성 모델을 개발
  - 여러 차례 실험하며 가장 이상적으로 퀴즈를 생성하는 Prompt를 개발
  - 비용을 고려하여 서비스에서는 GPT-3.5-Turbo 모델로 퀴즈를 생성하기로 함

### 서비스 구현 예시 : [Figma Web](https://www.figma.com/proto/1XoHRV91hfCMLNoS1yM1GO/bigkids_first_draft-(Community)?type=design&node-id=215-213&t=Vk5M39iCfRC9BdGt-0&scaling=min-zoom&page-id=0%3A1&starting-point-node-id=215%3A213)

#### 🏆 2023 뉴스빅데이터 해커톤 대상 수상
