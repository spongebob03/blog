---
title: "Regularization"
date: 2022-07-04T22:18:06+09:00
draft: false
tags: ["ml", "dls"]
description: overfitting을 막는 방법 중..💊
libraries:
- mathjax
---
## 가중치 규제?
{{<youtube 6g0t3Phly2M>}}

복잡한 모델을 좀 더 간단한 모델로 만들어 과적합될 가능성을 낮추는 방법. 간단한 모델이란 적은 수의 매개변수를 가진 모델을 말한다.

- L1 regularization(=L1 norm): 모든 가중치 w의 절대값 합계를 비용 함수에 추가. 
$$ L1_{regularization} = \frac{\lambda}{m} \Sigma |W| = \frac{\lambda}{2m} ||w||_1 $$

- L2 regularization(=L2 norm): 모든 가중치 w의 제곱합을 비용 함수에 추가. 
$$  L2_{regularization} = \frac{1}{m} \Sigma L(\hat{y}^{(i)}, y^{(i)}) + \frac{\lambda}{2m} ||w||^2_2 $$

`lambd`는 규제의 강도를 정하는 하이퍼파라미터. `lambd`가 크다면 모델이 훈련 데이터에 대해서 적합한 매개 변수를 찾는 것보다 규제를 위해 추가된 항들을 작게 유지하는 것을 우선한다는 의미. (`lambda`는 파이썬 키워드이기 때문에 수식에서 람다는 `lambd` 변수명을 주로 사용)

L2 규제는 L1 규제와는 달리 가중치들의 제곱을 최소화하므로 w의 값이 완전히 0이 되기보다는 0에 가까워지기는 경향을 띈다. L1 규제는 w가 희미해져(w벡터에 0이 많음) 어떤 특성들이 모델에 영향을 주고 있는지를 정확히 판단하고자 할 때 유용하지만 별로 도움이 안되서 많이는 안쓰임. 경험적으로는 L2 규제가 더 잘 동작하므로 L2 규제를 더 많이 쓴다. 

L2 규제는 "가중치 감쇠(weight decay)"라고도 불린다.

## Technique
### 💊 Data Augmentation
![](https://wikidocs.net/images/page/164688/Fig_01.png)
오버 피팅은 더 많은 훈련 데이터를 가져와서 해결할 수 있다. 하지만 훈련 데이터 더 가져오는 거는 비싸거나 수집하기 어렵다. 데이터 증강은 데이터를 추가하는 것보다 덜 효과적이어도 비싸지 않고, 알고리즘에 데이터 더 제공하고 일반화를 통해 과적합을 줄일 수 있다.
### 💊 Early Stopping
J 비용함수를 최적화시키는 진행 중에 자르는 것 + 과적합 방지
#### 단점
머신러닝 과정은 w, b를 찾고 J(w, b) 최저 비용 구하기, overfit 방지 이 두 과정이다. 이 두 과정은 각각 따로 생각하고 맞는 방법을 선택한다. 하지만 early stopping은 이 두 과정을 묶어버린다. 이 두 과정을 별개로 각각 단독으로 풀 수 없게 되고, 따라서 시도해야 할 방법이 더 **복잡**해진다. 
#### 대안
L2 regularization으로 lambda값 시도하는 방법이 대안이다. 하지만 비용이 더 많이 들 수 있다.
### 💊 Dropout
👉  [Dropout 포스트]({{< ref "./Dropout" >}} "Dropout")

## 참조
[과적합을 막는 방법들](https://wikidocs.net/61374)