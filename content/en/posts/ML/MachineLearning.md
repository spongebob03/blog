---
title: "Machine Learning"
date: 2022-07-02T22:19:06+09:00
draft: true
tags: ["ml"]
description: 인공지능 > "머신러닝" > 딥러닝
---
## 머신러닝

## 데이터
- Train: 모델 학습하는 용도
- Validation: 모델 성능 조정하기 위한 용도. 다양한 알고리즘 테스트하고 어떤 알고리즘이 더 잘 작동하는지 확인하는 용도
- Test: 모델 테스트하는 용도. 최종 모델이 주어졌을 때 얼마나 잘 수행되는지 추정하는 용도
일상에서 비유하자면 훈련 데이터는 문제집, 검증 데이터는 모의고사, 테스트 데이터는 수능

- 파라미터: 가중치, 편향 (학습을 하는 동안 값이 계속 변하는 값)
- 하이퍼파라미터: 모델의 성능에 영향을 주는, 사람이 값을 지정하는 변수 -> 딥러닝으로 해결
  - learning rate
  - number of iterations
  - number of hidden unit, layer
  - activation function
  - minibatch size
  - regularization

## 문제 유형
### 회귀 문제 Regression
어떠한 연속적인 값의 범위 내에서 예측 값이 나오는 경우
### 분류 문제
Logistic Regression이 쓰인다
#### 이진 분류 Binary Classification
#### 다중 클래스 분류 Multi-class Classification

## 머신러닝 유형
### 지도 학습 Supervised Learning
### 비지도 학습 Unsupervised Learning
### 자기지도 학습 Self-supervised Learning