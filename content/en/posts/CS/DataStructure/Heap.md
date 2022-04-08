---
title: "Heap"
date: 2022-04-06T12:12:26+09:00
draft: false
tags: ["자료구조", "힙"]
---
{{<boxmd>}}
🎄 특별한 이진 트리
완전 이진 트리에서 부모 노드가 자식 노드 값보다 같거나 작은(`MinHeap`)/큰(`MaxHeap`) 경우를 만족한다. 현재 남아있는 원소들 중 최대값 or 최솟값을 빠르게 **계속** 얻고 싶은 경우에 유용

{{</boxmd >}}
- heap만드는 데 **O(NlogN)** 소요
- 이후 삽입, 삭제 **O(logN)**
- <ins>최소값 or 최대값 탐색</ins> **O(1)**

## 구현

{{< expand "heapify">}}
{{< /expand >}}

{{< expand "insert">}}
{{< /expand >}}

{{< expand "remove">}}
{{< /expand >}}