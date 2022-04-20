# clustering-startup-M&A

## 연구 목적: provide data-driven taxonomy for M&A patterns

### data
- 출처: https://crunchbase.com
- M&A records (raw/M&A Records.csv)
  - 2011~2021년 사이의 인수합병 거래기록을 수집
  - 두 기업의 설립년도가 2000년 이후인 경우로 제한  
  > 수집 목록  
  ![1](https://user-images.githubusercontent.com/46666833/164212510-3a9a4db2-b5db-4f45-9b15-e54723c2bdc1.PNG)

- Company Information (raw/CompanyInfo.csv)
  - 설립년도가 2000년 이후인 기업의 특성 수집  
  > 수집 목록  
  ![2](https://user-images.githubusercontent.com/46666833/164213214-30cefeae-5a25-4159-874b-860be03f4973.PNG)
  ![3](https://user-images.githubusercontent.com/46666833/164213227-50d17da3-979a-4bf3-a54a-c085c0abb2cc.PNG)
  ![4](https://user-images.githubusercontent.com/46666833/164213235-262e1666-7932-4973-bdcb-dd5b3dbb0ad2.PNG)

### process
#### 1. Preprocessing
- M&A Records의 피인수, 인수 기업의 이름과 Company Information의 기업 이름을 매칭시킨 Master data 구축 -> Master.xlsx
  => 21,107개의 거래 기록 기반 기업 특성 data
- 데이터 축소
  - 특정 컬럼에서 결측치가 있는 경우 제거
    - 피인수 기업과 인수 기업 특성 중 ‘Industry Groups’, ‘Industries’, ‘Headquarters Location’, ‘Number of Employees’ 값이 없는 경우 제거
    - 21,107개 → 16,785개
  - 기업 나이가 음수인 경우 제거
    - ‘인수합병 년도 - 기업 설립 년도’ 가 음수인 경우 제거
    - 16,785개 → 16,697개
  - 전체 컬럼에서 결측치가 너무 많은 경우 제거
    - 기업 특성 변수 (피인수 기업 + 인수 기업) 34개 중 결측치가 있는 변수는 22개
    - 최소 11개 이상의 변수가 있어야 한다고 판단
    - 16,697개 → 11,070개 (data9.xlsx)
 - 카테고리 별 변수 축소
   - 기업 기본 정보
     - Number of Founders 만 결측치 존재 -> 결측치는 설립자가 1명이라 가정하고 1로 대체
   - 인지도
     - 가장 결측치가 적은 Number of Articles 만 사용, 결측 값은 기사 언급 수가 없다고 가정하고 0으로 처리
   - 기반 시설
     - 결측 값은 없다고 가정하고 0으로 처리
     - Number of Apps 는 사용 X
   - 펀딩 상태
     - Estimated Revenue Range, Number of Funding Rounds, Total Funding Amount Currency (in USD) 만 사용, 결측 값은 0으로 처리
   - 투자 규모
     - Number of Investors 만 사용, 결측 값은 투자자가 없다고 가정하고 0으로 처리

#### 2. Similarity & Difference
- Similarity 
  - 산업 분류 간 유사도
  - 지역 간 유사도   
  ![1](https://user-images.githubusercontent.com/46666833/164215988-a660f66f-40c9-438a-9f1e-3b1dc1febe61.PNG)

- Difference
  - 두 기업 변수 사이의 차이 비율  
  ![2](https://user-images.githubusercontent.com/46666833/164216053-fc83b8ae-b934-4a50-a781-4635c334ee37.PNG)

#### 3. Clustering & ANOVA Test
- K-means clustering 활용 (k=6, silhouette score = 0.4690)
- 해석에 유의미한 변수를 추출하기 위해 ANOVA Test 실시

#### 4. Result
- 5개의 클러스터 도출, 유의미한 변수들로 해석
  - Technology-Focused Field Expansion
  - Location-Based Strengthening competitiveness
  - Need Based & Scale up
  - For Synergy
  - Potential focused

### Conclusion
- 실제 M&A 기록을 기반으로 전반적인 M&A 거래 패턴을 파악함
- 기존 설문 중심 M&A 분석의 한계점을 극복
- 이를 통해 기업은 잠재적 M&A 파트너를 선정하는 방법과 성공적인 M&A 유형을 식별하는 방법에 대해 시사점을 얻을 수 있음

한계)
- 기업이 데이터베이스에 등록할 때 원하는 데이터만 등록할 수 있기 때문에 데이터 상의 한계점이 존재함
- M&A는 업종에 따라 다르게 이루어지기 때문에 특정 산업 분야를 기반으로 한 분석이 필요함
- 정확한 M&A 동기는 파악할 수 없음
