# 흡연 여부에 따른 건강 지표 차이 분석

건강검진 데이터로 흡연자와 비흡연자 사이의 건강 지표 차이를 통계적으로 검증한 분석 프로젝트입니다.

- **기간:** 2026.01
- **형태:** 팀 프로젝트 4명 (이 저장소는 개인 분석 파트)
- **담당:** 결측치 처리·전처리, 파생 변수 설계, 시각화, 가설 검정

<br>

## 분석 배경

흡연은 여러 건강 지표에 영향을 준다고 알려져 있지만, 실제 건강검진 데이터에서도 그 차이가 나타나는지 확인할 필요가 있었습니다.
흡연 여부(`label`)가 포함된 건강검진 데이터 7,000건으로 흡연자와 비흡연자의 지표 차이를 검증했습니다.

## Tech Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=flat-square&logo=scipy&logoColor=white)
![statsmodels](https://img.shields.io/badge/statsmodels-4B8BBE?style=flat-square)
![seaborn](https://img.shields.io/badge/seaborn-4C72B0?style=flat-square)
![Google Colab](https://img.shields.io/badge/Google%20Colab-F9AB00?style=flat-square&logo=googlecolab&logoColor=white)

## 분석 과정

### 1. 전처리

시력·공복 혈당·혈압·중성 지방에 각각 140건(2%)의 결측치가 있어, 변수 특성에 맞게 처리했습니다.

| 변수 | 처리 방법 | 이유 |
|---|---|---|
| 시력 | 최빈값 | 0.1 단위 이산값이 많음 |
| 혈압 | 중앙값 | 이상치 영향 최소화 |
| 공복 혈당 | 평균값 | 결측 비율이 2%로 낮아 대체값에 따른 분포 변화가 작음 |
| 중성 지방 | 나이대별 평균값 | 나이대마다 분포가 달라 전체 평균은 왜곡 위험 |

### 2. 파생 변수

임상 기준에 맞춰 구간·위험군 변수를 만들었습니다.

- 체중 상태 (BMI 기준 저체중·정상·과체중·비만), 나이대
- 중성 지방 상태 (정상 150 미만 · 경계 · 위험 200 이상)
- 저밀도지단백 위험군 (130 mg/dL 이상), 빈혈 여부 (헤모글로빈 12 g/dL 미만), 시력 상태

### 3. 시각화

흡연 여부별 인원, BMI·헤모글로빈 분포, 중성 지방과 체중 상태 박스플롯, 변수 간 산점도·상관관계 히트맵·pairplot으로 분포와 관계를 확인했습니다.

### 4. 가설 검정 (유의수준 5%)

| 가설 | 지표 | 검정 방법 | 결과 |
|---|---|---|---|
| 1 | 중성 지방 | t-검정, OLS 회귀 | 흡연자가 평균 **+36.88 mg/dL** 높음 (p < 0.001) |
| 2 | 충치 | 카이제곱 검정, OLS 회귀 | 흡연자의 충치 발생률이 **+8.5%p** 높음 (19.6% → 28.2%, p < 0.001) |
| 3 | 저밀도지단백(LDL) | 이원 분산분석 | 흡연 여부(p = 0.013)·나이대·상호작용 효과 모두 유의 |

## 결론

- 흡연자는 비흡연자보다 중성 지방이 높고 충치 발생률도 높아, 흡연이 대사 지표와 구강 건강 모두와 관련이 있음을 확인했습니다.
- LDL은 흡연 여부와 나이대가 함께 영향을 주며, 흡연의 영향 크기가 나이대마다 달랐습니다.

## 회고

- **인사이트:** 결측치를 어떻게 채우느냐에 따라 분석 결과가 달라질 수 있음을 체감했습니다.
- **한계:** 흡연 여부 하나로 비교해 성별·연령 같은 교란 변수를 통제하지 못했고, 그룹 평균 대체도 하나의 값으로 채우는 방식이라 분산이 줄어듭니다. 또한 공복 혈당처럼 꼬리가 긴 변수는 평균보다 중앙값 대체가 더 적절했을 수 있습니다.
- **개선 방향:** 다변량 회귀로 교란 변수를 함께 넣어 검증하고, 결측 제거·그룹 평균·회귀 대체로 각각 검정해 결론이 유지되는지 확인하고 싶습니다.

## 파일 구성

```
├── .gitignore                      # 데이터(csv) · 체크포인트 제외
├── README.md
└── smoking_health_analysis.ipynb   # 전처리 · 시각화 · 가설 검정
```

> 데이터 파일은 포함하지 않았습니다.