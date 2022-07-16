---
title: "Loss Function"
date: 2022-07-04T22:19:06+09:00
draft: false
tags: ["ml"]
description: cost function
libraries: 
- mathjax
---
## 손실 함수
{{<boxmd>}}
신경망 학습에서 **현재의 상태를 표현하는 지표**(손실함수)를 기준으로 최적의 가중치 매개변수의 값을 탐색한다
{{</boxmd>}}
## SSE
오차제곱합 Sum of Squares for Error
$$ E = \frac{1}{2}\displaystyle \sum_{k}(y_k - t_k)^2$$
각 원소의 출력(추정치)과 정답 레이블의 차를 제곱한 후, 그 총합을 구한다.
```python
def sum_squares_error(y, t):
    return 0.5 * np.sum((y-t)**2)
```
## MSE
평균제곱오차 Mean Squared Error
$$ MSE = \frac{1}{N}\displaystyle \sum_{k}^N (y_i - \hat{y}_i)^2$$
예측값과 정답 레이블의 차를 제곱한 후 그 총합의 평균을 구한다.
```python
def mean_squared_error(y, t):
    return (1/len(y)) * np.sum((y-t)**2)
```
## RMSE
평균제곱근오차 Root Mean Squared Error
$$ RMSE = \sqrt{\displaystyle \sum_{i=1}^n \frac{(\hat{y}_i - y_i)^2}{n}}$$
```python
def root_mean_squared_error(y, t):
    return np.sqrt((1/len(y)) * np.sum((y-t)**2))
```
## cross entropy error
교차 엔트로피 오차 
$$ E = -\displaystyle \sum_{k}t_k\log y_k$$
t는 원-핫 인코딩되어 있어 정답일 때의 추정의 자연로그를 계산한다. 따라서 정답일 때의 출력이 전체 값을 정한다. 
```python
def cross_entropy_error(y, t):
    delta = 1e-7
    return -np.sum(t * np.log(y + delta))
```
### 엔트로피?
하나의 변수가 가지는 확률 분포의 불확실성