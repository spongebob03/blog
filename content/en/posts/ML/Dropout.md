---
title: "Dropout"
date: 2022-07-05T21:54:28+09:00
draft: false
tags: ["ml", "regularization", "딥러닝"]
description: regularization 기법 중...
---
{{<youtube ARq74QuavAo>}}
복잡한 모델이 간단한 모델보다 과적합될 가능성이 높다. 그리고 간단한 모델은 적은 수의 매개변수를 가진 모델. 복잡한 모델을 좀 더 간단하게 하는 방법으로 가중치 규제(Regularization)가 있습니다.

무작위로 신경망의 유닛 누락시킨다 -> 더 작은 신경망 사용하는 것과 같다 
-> regularization 효과 -> 과적합 예방
## 원리
{{<boxmd>}}
1. 무작위로 입력이 제거될 수 있다
2. 한가지 특성에만 의존하면 안되겠다!
3. 한 특성에 비중 두는 것 피하기
4. 입력 노드들에 비중 나눠줌 (가중치 분산)
5. 가중치의 squared norm 줄이는 효과
6. overfitting 방지 👍
{{</boxmd>}}
L2 정규화와 비슷하지만 다른 입력값의 스케일에 따라 더 변형한다는 차이가 있다.
주로 컴퓨터 비전에서 입력 크기 너무 크기 때문에 많이 쓰인다.

## 단점
비용함수 J가 모든 반복에서 잘 정의되지 않는다
**:shrug:Why?**
무작위로 노드를 취소하다보니 경사하강법의 성능을 이중 확인한다면 실제 다시 확인하기 어렵기 때문
