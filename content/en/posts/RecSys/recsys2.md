---
title: "주요 추천 알고리즘"
date: 2021-11-19T22:09:48+09:00
draft: false
tags: ["추천시스템"]
categories: ["추천시스템"]
description: 
---
## 1. Content-Based filtering: CB
> 비슷한 아이템을 기반으로 추천

- 도메인 지식 필요
- 아이템 간의 유사도 측정하여 추천
- 모든 이에게 동일한 아이템을 추천
- 보통 아이템, 사용자 수가 적다
- hand-engineered features에서만 성능 좋음
- 텍스트 처리 필요하기도 함(NLP) - _예.작가의 다른 책_

핵심
1. 아이템들을 어떤 정보를 기준으로 서술할 건가
2. 유사소 계산 방법 정하기 (코사인, 피어슨...)
## 2. Collaborative Filtering: CF
> 기본적으로 다른 사용자들의 정보를 이용하여 취향 예측

### 1) 메모리 기반 (=Nearest Neighbor based CF)
#### - 👨‍👩‍👧‍👦 사용자 기반
나와 비슷한 평점 패턴을 보이는 **사람**들을 찾아서 그 사람들이 높게 평가한 아이템 추천

- 나와 다른 사람 어떻게 표현할 것인가
- 나와 비슷한 사용자를 어떻게 찾을지가 중요
    - 사용자 프로파일 정보 구축
    - 프로파일 정보간의 유사도 계산
#### - 🎨 아이템 기반
아마존에서 논문으로 발표
유사도 비교 → **평점**의 패턴이 비슷한 **아이템**들을 찾아서 그걸 추천하는 방식
아이템의 수가 보통 사용자 수보다 적다 → 평점의 수가 평균적으로 더 많고 계산량이 작다 → 사용자 기반보다는 덜 sparse → 사용자 기반 협업 필터링과 비교해 더 안정적이며 좋은 성능을 보임
### 2) 모델 기반
- 넷플릭스 프라이즈 컨테스트 때 고안된 추천 방식
- 사용자 아이템 행렬에서 비어있는 평점들을 SGD를 사용해서 채우는 방식
- SVD(Singular Vector Decomposition)을 사용해서 구현
    - 요즘은 딥려닝의 오토인코더를 사용
- 평점을 포함한 다른 사용자 행동을 예측하는 방식으로 진화
### 3) 사용자 행동 기반
- 사용자 행동(아이템 클릭 or 구매 등) 정보를 기반으로 추천
    - 사용자, 아이템에 대한 부가 정보 필수
    - 구현 간단하지만 아주 유용
- 사용자 행동을 예측하는 추천 (클릭 or 구매)
    - 모델링을 통해 사용자와 아이템 페어에 대한 클릭 확률 등의 점수 계산 가능
        - 의사 결정 트리나 딥러닝등이 사용 가능
        - 유데미에서 사용했던 방식
### 4) Latent Factor CF
사용자-아이템 평점 행렬 데이터만을 이용해 말 그대로 '잠재 요인'을 끄집어 내는 것
- Matrix Factorization
- 우연한 추천 기능
# 3. 다수의 알고리즘: 다양한 방식 조합
# 4. 지도학습 방식
 어떤 기준으로 추천을 하느냐가 가장 중요 - 머신러닝의 레이블 정보!

힌트
- 명시적 힌트: 리뷰점수, 좋아요
- 암시적 힌트: 클릭, 구매, 소비
  - 클릭보다는 구매가 더 좋은 힌트, 소비 여부도 좋은 힌트다
  - 클릭은 노이즈가 있지만 장점은 관심없는 아이템들의 파악이 쉽다
  - 자세한 사용자 행동 정보의 수집, 저장과 가공 먼저 필요