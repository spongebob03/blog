---
title: "Normalization"
date: 2022-07-08T22:18:06+09:00
draft: true
tags: ["ml", "dls"]
description: setting up your optimization problem
libraries:
- mathjax
---
## 과정
1. 평균 빼거나 0으로 만들기
2. 분산을 normalize
3. 좀 더 고르게 분포하게 된다!
## 왜 쓰는가?
정규화 안하면 비용함수 매우 찌그러지거나 길쭉하다. 여기서 경사하강법을 사용하면 아주 작은 학습 속도(learning rate)를 써야할 수 도 있는데 이는 비용과 시간이 커진다.
따라서 normaliztion은 비용 함수가 빠르게 최적화되도록 만든다.
## Tip
훈련셋 척도 조정할 경우 동일한 μ, σ 사용해서 시험셋으로 normalize해야함