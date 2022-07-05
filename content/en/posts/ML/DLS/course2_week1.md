---
title: "Improving Deep Neural Network"
date: 2022-07-04T22:17:06+09:00
draft: false
tags: ["ml", "dls"]
description: DLS Course2
---
## Dataset

Dataset ➡️ train / dev / test

- 데이터셋 비중
이전에는 데이터셋 비중을 60% / 20% / 20% 을 이상적으로 생각했다. (10000개 정도 데이터에서는 이 비중이 이상적이지만)
1000,000 이상의 빅데이터라면 1000,000/ 10000/ 1000. 즉, 98% / 1% / 1%. 
더 큰 데이터면 훈련 비중을 높이고 검증, 테스트 데이터셋 비중을 더 줄일 수 있다.
- 데이터 형태
같은 배포에서 들어온 데이터로 하면 성능 더 높일 수 있다.

## Bias / Variance
example
|훈련 데이터셋 오류율|검증 데이터셋 오류율||
|:-:|:-:|:-:|
|1%|11%|high variance(overfitting)|
|15%|16%|high bias(underfitting)|
|15%|30%|high bias & high variance|
|0.5%|1%|low bias & low variance|

## How to deal with bias, variance?
- high bais?
  - **Bigger network**
  - Train loger
  - NN 구조 손보기

- high variance?
  - **More data**
  - Regularization
  - NN 구조 손보기