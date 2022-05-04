---
title: "다익스트라 알고리즘"
date: 2022-05-04T21:15:37+09:00
draft: false
description: "최단경로 구히가: Dijkstra algorithm"
tags: ["알고리즘", "그래프", "최단경로"]
categories: ["알고리즘"]
libraries:
- mathjax
---
## Dijkstra
{{<boxmd>}}
<ins>특정 시작점</ins>에서 다른 모든 정점으로 가는 최단 거리를 <ins>각각</ins> 구해주는 알고리즘
{{</boxmd>}}
**A도시**에서 **D도시**로 가는 <ins>최단 비용</ins>(거리, 시간, 돈)을 구하는 문제에 적합한 알고리즘.  
⚠️ 음수 가중치 간선이 있는 그래프에서는 쓸 수 없다.

||시간복잡도|
|:-:|:-:|
|우선순위 큐⭕️|$O(ElogV)$|
|우선순위 큐❌|$O(V^2)$|
## 구현
```python
import heapq

INF = int(1e9)

def dijkstra(start):
    queue = []
    heapq.heappush(queue, (0, start))
    distance[start] = 0

    while queue:
        dist, node = heapq.heappop(queue)

        for v, c in graph[node]:
            cost = dist + c
            if cost < distance[v]:
                distance[v] = cost
                heapq.heappush(queue, (cost, v))
```

{{< expand "입력 및 실행 코드">}}
```python
n, m = map(int, input().split()) # 노드, 간선 개수
start = int(input()) # 출발 노드

graph = [[] for _ in range(n+1)]
distance = [INF] * (n+1)

# 간선 정보
for _ in range(m):
    source, dest, cost = map(int, input().split())
    graph[source].append((dest, cost))

dijkstra(start)

for i in range(1, n+1):
    if distance[i] == INF:
        print("INF")
    else:
        print(distance[i])
```
{{< /expand >}}