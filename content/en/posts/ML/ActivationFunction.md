---
title: "Activation Function"
date: 2022-07-02T22:19:06+09:00
draft: false
tags: ["ml"]
description: 활성함수
libraries:
- mathjax
---
신경망에서 활성 함수로 쓰이는 비선형 함수들 🧨
## Sigmoid
![](/images/ML/sigmoid.png)
$$ h(x) = \frac{1}{1 + e^{-x}} $$
```python
def sigmoid(x):
    return 1 / (1 + np.exp(-x))
```
입력이 작을 떄의 출력은 0에 가깝고 입력이 커지면 출력이 1에 가까워진다. 
또한 출력이 클수록 기울기가 0에 가깝다. 
## ReLU
![](/images/ML/relu.png)
$$ h(x) = 
\cases{
\rm x \qquad \text{if }\ x\gt 0\cr
\rm 0 \qquad \text{if }\ x\le 0
}  $$
```python
def relu(x):
    return np.maximum(0, x)
```
입력이 0을 넘으면 그 입력을 그대로 출력하고 0 이하면 0을 출력한다.
## Leaky ReLU
![](/images/ML/leaky_relu.png)
$$ h(x) = 
\cases{
\rm x \qquad \text{if }\ x\gt 0\cr
\rm 0.01 \times x \qquad \text{if }\ x\le 0
}  $$
```python
def leaky_relu(x):
    return np.maximum(0.01*x, x)
```
ReLU에서 0이하의 입력에 모두 0으로 출력하는데 이로 인해 발생하는 손실이 발생할 수  있다.  
반면 Leaky ReLU는 0이하의 입력을 0에 "근접"한 매우 작은 값으로 출력한다.
## Tanh
![](/images/ML/tanh.png)
$$ h(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}} $$
```python
def tanh(x):
    return (np.exp(x) - np.exp(-x)) / (np.exp(x) + np.exp(-x))
```
## 🤷 왜 비선형 함수를 쓰는가?
선형함수를 쓴다면 층을 아무리 깊게 해도 하나의 층과 똑같을 수 있다...도루묵
예) `h(x) = cx` 로 3개 층을 쌓으면 `y(x) = h(h(h(x))) = c * c * c * x = a * x`

따라서 **층을 쌓은 효과를 얻기 위해서는 비선형 함수를 활성 함수로 써야한다.**

## 참고
밑바닥부터 시작하는 딥러닝