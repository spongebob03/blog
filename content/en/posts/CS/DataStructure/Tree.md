---
title: "Tree"
date: 2022-04-05T12:09:48+09:00
draft: false
tags: ["자료구조", "트리"]
---
## 트리
{{< boxmd >}}
🌲 정점끼리 <ins>전부 연결</ins>되어 있으면서 사이클이 존재하지 <ins>않는</ins> 그래프
{{< /boxmd >}}
{{< figure src="/images/CS/tree_sample.jpeg" caption="원소의 구성을 정말 나무처럼 표현한 예시로 영화 '해리포터' 속에서 나왔던 시리우스 블랙의 가계도가 생각났다. 자료구조의 트리는 나무가 거꾸로 있는 형태이므로 가문의 선조가 가장 위에 있도록 원본 이미지를 회전했다." >}}

⬆️위와 같은 가계도 모양을 생각하면 노드는 가문의 각 구성원이며 왜 상위 노드를 `부모 노드`, 하위 노드가 `자식 노드`라고 불리는지 쉽게 이해할 수 있다. 이 예시로 <ins>트리 용어</ins>들을 일상적으로 비유해 본다면 `루트 노드`는 가문의 선조, `차수`는 각 구성원에서 뻗어나간 가지 수로 몇 명의 자식이 있는지, `깊이`는 현재 구성원이 선조로부터 몇 세대 내려왔는지, `높이`는 마지막 후손의 `깊이`이다.
## <ins>이진</ins> 트리
{{< boxmd >}}
🌲 자식 수가 <ins>최대 2개</ins>인 트리
{{< /boxmd >}}
### 이진트리 탐색
- 전위 탐색: **부모** - 좌 - 우
- 중위 탐색: 좌 - **부모** - 우
- 후위 탐색: 좌 - 우 - **부모**

## 이진<ins>탐색</ins>트리

- **왼쪽 방향**에 있는 노드들은 전부 부모 보다 값이 <ins>작아야 함</ins>
- **우측 방향**에 있는 노드들은 전부 부모 보다 값이 <ins>커야만 한다</ins>
- 삽입
- 삭제
    

## <ins>균형잡힌</ins> 이진 탐색 트리
{{<boxmd>}}
삽입, 삭제가 일어나는 순간 루트 노드와 주위에 있는 노드를 회전 등의 작업을 통해 적절하게 조절 → 트리의 높이를 최소화
{{</boxmd>}}
- 트리 높이를 항상 logN으로 유지할 수 있다
- 삽입, 삭제, 탐색 시간 O(logN)

## <ins>완전</ins> 이진 트리

- 모든 값이 왼쪽에서 순서대로 차 있다
-  모든 노드에 대해 부모 노드가 자신의 자식보다 같거나 큰 or 작은 경우 👉  [Heap]({{< ref "./Heap" >}} "Heap")