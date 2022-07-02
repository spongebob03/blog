---
title: "프림 알고리즘"
date: 2022-05-15T23:02:38+09:00
draft: false
libraries:
- mathjax
description: Prim Algorithm
tags: ["알고리즘", "그래프", "최단경로"]
categories: ["알고리즘"]
---

## Prim Algorithm
{{<boxmd>}}
크루스칼이 최소비용 **간선**을 선택해 나간다면, 프림 알고리즘은 최소비용 간선이 있는 **정점**을 선택해 나간다.
{{</boxmd>}}

||시간복잡도|
|:-:|:-:|
|우선순위 큐|$O(ElogV)$|
|인접행렬|$O(V^2)$|

{{<youtube cplfcGZmX7I>}}

## 구현
```python
import heapq

INF = int(1e9)

def prime(start):
    queue = []
    heapq.heappush(queue, (0, start))
    distance[start] = 0

    while queue:
        dist, u = heapq.heappop(queue)

        for v, c in graph[u]:
            if c < distance[v]:
                distance[v] = c

n, m = map(int, input().split()) # 노드, 간선 개수
start = int(input()) # 출발 노드

graph = [[] for _ in range(n+1)]
distance = [INF] * (n+1)

# 간선 정보
for _ in range(m):
    source, dest, cost = map(int, input().split())
    graph[source].append((dest, cost))

prime(start)

print(distance)
```