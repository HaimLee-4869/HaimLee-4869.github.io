---
title: "Mingle-K - 데이터 기반 K리그 팬 성향·소셜 매칭 플랫폼"
slug: "mingle-k"
draft: false
roles: "Data, AI, Fullstack"
image: /pic/mingle.png
emoji: "⚽"
badges:
  - "Python"
  - "PCA"
  - "K-Means"
  - "RAG"
links:
  live: ""
  github: ""
  pdf: "/pdf/서울시립대_데이터경진대회_-_JBIG.pdf"
  news: ""
summary: "K리그 선수 행동·신체 데이터를 전처리하고 PCA·K-Means로 4개 성향 축을 분석해 16가지 FBTI(Football Type Indicator)를 설계했습니다. 분류 결과를 RAG 기반 AI 코치와 React Native 앱에 연결해 선수 성향 해설, 유사 선수 추천, 팬 매칭까지 제공하는 MVP를 구현했습니다."
---

K리그 팬층이 확대되면서 경기 결과뿐 아니라 선수 개인의 캐릭터와 플레이 스타일을 소비하는 팬이 늘고 있다는 점에서 시작했습니다.

기존 득점·도움·패스 성공률 등의 지표는 선수가 얼마나 잘했는지는 보여주지만, ‘어떤 스타일의 선수인가’를 팬이 직관적으로 이해하기에는 한계가 있다고 판단했습니다.

이에 제공된 경기 데이터를 팬이 이해할 수 있는 새로운 성향 지표로 변환하고, 이를 팬 간 연결까지 확장하는 서비스를 기획했습니다.

## Raw Data를 팬의 언어로 바꾸다

선수의 행동 이벤트 데이터와 기본 정보, 신체 데이터를 결합했습니다.

2024 시즌에서 유의미한 활동을 한 선수를 대상으로 분석하기 위해 **500분 이상** 출전 선수만 필터링하고, 축구 행동을 설명할 수 있는 **파생변수**를 직접 설계했습니다.

대표 feature:

- `Duel Success Rate`
- `Avg dx`
- `Central Zone Ratio`
- `Box Entry Per90`
- `Forward Pass Ratio`
- `Side Zone Ratio`
- `Through Pass Per90`
- `Total Carry Distance`
- `Event Intensity`
- `Set Piece Per90`

## FBTI — 4개 축, 16가지 플레이 유형

복잡한 축구 데이터를 MBTI처럼 직관적인 구조로 전달하기 위해 **FBTI(Football Type Indicator)**를 설계했습니다.

4개 축:

### P / T — Physical vs Technical

신체적 강인함과 기술적 정교함

### A / S — Aggressive vs Strategic

공격적 전진성과 전략적 안정성

### W / C — Wide vs Central

경기장 폭 활용과 중앙 밀도

### D / O — Dynamic vs Organized

역동적 활동량과 전술적 조직력

각 축에 관련 feature를 선택하고 **PCA**를 통해 차원을 축소한 뒤 **K-Means**(`k=2`)로 두 군집을 분류했습니다.

4개 이진 축의 조합으로 **2^4 = 16가지** FBTI 유형을 구성했습니다.

## 클러스터링 결과를 다시 검증

군집 결과를 그대로 서비스에 적용하지 않고 실제 축구 데이터의 특성과 일치하는지 추가 검증했습니다.

### 논리적 지표 검증

Physical 유형과 Technical 유형의 키, 몸무게, 경합 성공 지표, 패스 성공률 등을 비교해 각 유형의 정의가 실제 데이터 특성과 일치하는지 확인했습니다.

### Silhouette Score

4개 축의 군집 분리 정도를 **Silhouette Score**로 확인했습니다.

- P vs T: `0.3848`
- A vs S: `0.3654`
- W vs C: `0.5664`
- D vs O: `0.4214`

위 값은 군집 품질을 확인하기 위한 내부 검증 지표로 사용했습니다.

### 포지션과의 연관성

FBTI 각 축과 선수 포지션 사이의 연관성을 **카이제곱 검정**으로 확인했습니다.

네 축 모두 `p-value < 0.05`가 관찰됐으며, **FBTI 축과 선수 포지션 사이에 통계적으로 유의한 연관성이 관찰됐습니다.**

## 데이터를 캐릭터와 이야기로

분석 결과가 숫자와 코드로 끝나면 라이트 팬에게 다시 어려운 데이터가 된다고 판단했습니다.

FBTI 유형과 선수의 소속팀·플레이 특징을 **Prompt Dictionary**에 매핑해 캐릭터와 설명으로 변환하는 **Dynamic Prompting** 구조를 설계했습니다.

P/T, A/S, W/C, D/O 유형에 따라 체격·표정·자세 등의 시각적 토큰을 선택하고, 소속 구단 정보를 결합해 생성형 AI가 사용할 prompt를 자동 조립하도록 구성했습니다.

## RAG 기반 AI 코치

FBTI는 팀이 개발한 독자적인 지표이기 때문에 일반 LLM이 해당 유형의 의미와 선수 데이터를 알고 있지 않습니다.

이를 해결하기 위해 분석된 선수별 FBTI와 상세 스탯을 **Knowledge Base**로 구성하고 **RAG 기반 AI 코치**를 구현했습니다.

사용자의 질문에서 FBTI 유형을 찾고, 관련 유형 정의와 대표 선수 데이터를 검색한 뒤, 해당 Context와 질문을 Gemini에 전달해 다음 정보를 제공하도록 구성했습니다.

- FBTI 성향 해설
- 유사 선수 추천
- 관전 포인트
- 응원 팀/스타일 추천

내부 분석 데이터를 Context로 제공해 답변의 근거성과 일관성을 높였습니다.

## 분석 모델을 모바일 MVP로

분석 Notebook에서 끝내지 않고 실제 모바일 서비스 MVP까지 연결했습니다.

- Frontend: **React Native**
- Backend: **FastAPI** (Python)
- AI: `Gemini 2.5 Flash` / RAG
- Data: Local CSV + Firebase

정적인 FBTI 분석 결과는 CSV 형태로 경량화해 조회하고, 사용자 인증·매칭 정보는 Firebase에서 관리했습니다.

FastAPI 서버에서는 정적 데이터 조회와 AI 호출 로직을 연결하고, React Native 앱에서 성향 진단 → 결과 확인 → 선수 추천 → AI 코치로 이어지는 사용자 흐름을 구현했습니다.

## 데이터 분석을 팬 경험으로 확장

FBTI를 단순한 재미용 테스트로 끝내지 않고 팬·구단·리그가 함께 활용할 수 있는 구조를 제안했습니다.

팬:

- FBTI 기반 정체성
- 유사 성향 팬 매칭
- 온·오프라인 팬 커뮤니티

구단:

- FBTI 기반 굿즈
- 팬 세분화
- 데이터 기반 CRM

리그:

- 신규 팬에게 이해하기 쉬운 Player Indicator
- 경기장 Mingle Event
- 중계 콘텐츠

위 항목은 실제 도입 성과가 아니라 향후 확장 시나리오로 설계한 내용입니다.

## 배운 점

이 프로젝트를 통해 데이터 분석의 결과는 정확한 모델을 만드는 것만으로 끝나지 않고 사용자가 이해할 수 있는 언어와 경험으로 번역되어야 실제 서비스 가치가 생긴다는 점을 배웠습니다.

선수 행동 데이터를 feature로 구조화하고 PCA·K-Means로 유형을 만든 뒤 다음 단계로 연결했습니다.

통계 검증  
→ 생성형 AI  
→ RAG  
→ FastAPI  
→ React Native 앱

**데이터 분석 → ML → AI 서비스 → 사용자 경험**으로 이어지는 End-to-End 서비스 개발 과정을 경험했습니다.
