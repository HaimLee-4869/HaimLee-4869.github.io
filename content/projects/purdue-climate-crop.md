---
title: "기후 변화에 따른 미국 옥수수 수확량 예측 & 작물 배분 최적화"
slug: "purdue-climate-crop"
draft: false
roles: "Data, ML, Optimization"
image: /pic/purdue.png
emoji: "🌽"
badges:
  - "Python"
  - "ML"
  - "Optimization"
links:
  live: ""
  github: ""
  pdf: ""
  news: ""
summary: "Purdue PAGE Program에서 1981~2015년 미국 2,644개 카운티의 기후·토양·수확량 데이터를 분석했습니다. 9개 모델을 비교해 LightGBM 공간 교차검증 R² 0.73을 확보하고, 29℃ 이상 극한고온의 영향을 분석했습니다. 이후 ML 예측값을 2,056개 카운티의 작물 배분 최적화 문제로 확장해 GA·SA·TS를 비교하고 전환비용과 기후손실 회복의 trade-off를 분석했습니다."
---

Purdue University PAGE Program에서 **기후변화에 따른 미국 작물 생산 리스크**를 분석하는 4인 팀 연구를 수행했습니다. 

단순히 미래 수확량 감소를 예측하는 것을 넘어서 “제한된 경작지에서 어떤 지역에 어떤 작물을 배분해야 기후변화 피해를 줄일 수 있는가”라는 질문을 확장하였습니다.

그래서 연구를 **ML 기반 수확량 예측 → 기후 영향 해석 → 작물 배분 최적화**로 연결했습니다.

## 35년 × 2,644개 카운티 기후·농업 데이터

**1981~2015년** 미국 **2,644개 카운티**를 대상으로 기온 노출 분포, 가뭄, 강수, 토양 특성과 실제 옥수수 수확량 데이터를 결합했습니다.

주요 데이터:

- ACDC Climate Dataset
- DSCI
- TerraClimate
- USDA NASS

성장기 184일의 기온을 1℃ 단위 **121개 temperature bin**으로 구성해 평균기온 하나가 아니라 어떤 온도 구간에 며칠 노출됐는지를 분석했습니다.

### 온도 분포의 구조적 제약 처리

121개 temperature bin의 합이 항상 184일로 고정돼 있기 때문에 perfect collinearity가 발생할 수 있었습니다. 그래서 reference bin 하나를 제거하고, 누락값은 `184 - 나머지 bin의 합`으로 복원하는 방식으로 전처리했습니다.

## 다중공선성 문제

인접 temperature bin 사이의 Pearson correlation이 **0.94 이상**으로 나타났습니다. 이 구조에서는 단순 OLS의 성능과 안정성이 크게 떨어질 수 있다고 판단해 다음과 같이 정규화 선형모델과 tree ensemble을 포함한 **9개 모델**을 비교했습니다.

- OLS
- Ridge
- Lasso
- ElasticNet
- Random Forest
- Gradient Boosting
- XGBoost
- CatBoost
- LightGBM

최종적으로 **LightGBM**이 가장 좋은 예측 성능을 보였습니다.

## 검증

같은 county의 지역적 특징이 train/test에 동시에 포함되면 모델이 지역 자체를 기억해 성능이 과대평가될 수 있다고 판단했습니다. 따라서 county 단위 Spatial Cross-Validation을 적용했습니다.

검증 결과:

- **Spatial CV R² ≈ 0.73**
- **Rolling-Origin temporal validation R² ≈ 0.656**

또한 Random Split을 사용할 경우 약 **+0.2 수준의 성능 inflation**이 발생할 수 있음을 확인했습니다. 높은 숫자를 만드는 것보다 실제 새로운 지역과 미래 시점에서도 모델이 작동하는지 검증하고자 했습니다.

## 평균기온보다 극한고온 노출이 중요

EDA에서 평균기온이 유사한 지역도 **29℃ 이상 극한고온**에 노출된 일수에 따라 수확량이 크게 달라질 수 있음을 확인했습니다.

28~31℃ 범위에서 threshold를 변화시키며 분석한 결과 해당 범위에서 수확량과 온도 노출의 관계가 안정적으로 나타났으며, 특히 29℃ 이상의 극한고온 노출이 증가할수록 수확량이 급격하게 감소하는 비선형 패턴을 확인했습니다.

## Feature Importance ≠ Causality

LightGBM에서 토양 pH 등 일부 토양변수가 높은 feature importance를 나타냈습니다. 하지만 토양 특성은 지역별 차이가 큰 변수이므로 카운티의 지역성을 대신 표현하는 proxy일 가능성이 있다고 판단했습니다.

따라서 **예측에서 중요한 변수 ≠ 수확량의 직접적인 원인**으로 구분해 해석했습니다.

## 지역·연도 효과를 통제한 극한고온 영향

카운티와 연도 **Fixed Effects**를 함께 통제한 모델에서도 극한고온의 부정적 영향이 유지됐습니다. 29℃ 이상 노출이 하루 증가할 때 옥수수 수확량이 약 1.67 bu/ac 감소하는 관계가 나타났고, 28~31℃ threshold 범위에서도 부정적인 방향이 일관되게 유지됐습니다.

완전한 인과관계를 증명한 결과가 아니라, county/year fixed effects를 통제한 모델에서도 극한고온의 부정적 영향이 유지된 것으로 해석했습니다.

## Prediction → Optimization

수확량을 예측하는 데서 연구를 끝내지 않고 ML 예측값을 실제 작물 배분 의사결정 문제의 입력값으로 사용했습니다.

- 대상: **2,056개 미국 county**
- 작물: Corn, Soybean, Wheat
- 의사결정: 각 county에 배분할 작물
- 제약: county별 총 경작지 면적 고정, 최소 식량 생산량, 작물 변경에 따른 switching cost

## ML 예측값을 목적함수로

카운티별 예측 수확량을 이용해 `margin per acre = crop price × predicted yield - production cost`를 계산했습니다.

목적함수는 전체 margin을 최대화하되 작물 전환에 필요한 **switching cost**를 함께 penalize하는 `전체 경작지 margin - λ × switching cost` 구조로 설계했습니다.

## Greedy baseline의 한계

먼저 switching cost를 고려하지 않고 각 county에서 margin이 가장 높은 작물을 선택하는 Greedy 방식을 baseline으로 구성했습니다.

**+2℃ 가정 기반 최적화 simulation**에서 약 30%의 경작지 변경과 약 23%의 warming loss recovery가 나타났습니다. 다만 switching cost를 무시한 theoretical ceiling에 가까웠습니다.

이에 실제 문제를 “어떻게 가장 적게 바꾸면서 최대한 많은 피해를 회복할 것인가”로 재정의했습니다.

## GA · SA · TS 비교

최적화 결과가 특정 알고리즘 하나에만 의존하지 않는지 확인하기 위해 **Genetic Algorithm, Simulated Annealing, Tabu Search** 세 가지 Metaheuristic을 구현해 비교했습니다.

비교 기준은 Solution Quality, Wall-clock Time, Stability였습니다. 동일한 조건에서 세 방법 모두 높은 품질의 해를 찾았으며, 표준 Tabu Search가 약 0.13초로 가장 빠른 결과를 보였습니다.

## 전환비용과 피해 회복의 Trade-off

switching cost coefficient λ를 변화시키며 경작지 변경 비율과 warming-loss recovery 사이의 trade-off curve를 분석했습니다. 약 **λ ≈ 42** 부근에서 curve의 knee point가 나타났습니다.

**+2℃ 가정 기반 최적화 simulation에서** 약 8%의 경작지 변경으로 약 17%의 warming loss recovery가 나타나는 절충안을 확인했습니다. 

## 최적점의 Robustness

λ search grid를 18개에서 60개로 세분화해 다시 분석했습니다. knee position은 약 **39.9~42.3** 범위에서 유지돼 parameter grid 변화에도 절충점이 크게 변하지 않는지 재검증했습니다.

## 식량안보 ↔ 기후적응 Trade-off

최소 corn 생산량 조건을 추가해 식량안보 수준을 높였을 때 작물 전환의 자유도가 감소하면서 warming-loss recovery도 낮아지는 패턴을 확인했습니다.

전역 제약이 추가되면 county별 의사결정이 서로 독립적이지 않게 되어 최적화 난도가 커졌으며, 단순히 가장 높은 margin을 찾는 것보다 **식량안보·기후적응·전환비용**을 동시에 고려해야 한다는 점을 확인했습니다.

## 배운 점

이 연구를 통해 예측 성능 자체가 의사결정은 아니라는 점을 배웠습니다.

2,644개 county의 기후·농업 데이터를 분석해 수확량을 예측하고, 데이터 누수 가능성을 고려해 공간·시간 교차검증으로 모델을 검증했습니다. 이후 ML 예측값을 2,056개 county의 작물 배분 문제에 연결해 전환비용과 식량안보라는 현실적인 제약까지 함께 분석했습니다.

이를 통해 **데이터 분석 → ML 예측 → 모델 검증 → 영향 해석 → Optimization → 정책 Trade-off**로 이어지는 데이터 기반 의사결정 과정을 경험했습니다.
