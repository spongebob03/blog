---
title: "플로이드 와샬"
date: 2022-05-04T21:30:53+09:00
draft: false
description: "최단경로 구히가: Floyd Warshall algorithm"
tags: ["알고리즘", "그래프", "최단경로"]
categories: ["알고리즘"]
libraries:
- mathjax
---

## Floyd Warshall
{{<boxmd>}}
그래프의 모든 노드 쌍에 대한 최단거리를 구하는 알고리즘
{{</boxmd>}}
모든 지점끼리의 거리를 구한다. 다이니믹 프로그래밍 기반이다. A->B로 다이렉트로 가는 경로와 A-><ins>k</ins>->B 경유해서 가는 경로를 비교해서 더 비용이 작은 경로의 값으로 갱신한다.
- 시간복잡도: $O(V^3)$
## 구현
```python
INF = int(1e9)

def floyd(graph, n):
    for k in range(1, n+1):
        for i in range(1, n+1):
            for j in range(1, n+1):
                graph[i][j] = min(graph[i][j], graph[i][k] + graph[k][j])
```

{{< expand "입력 및 실행 코드">}}
```python
n, m = map(int, input().split()) # 노드, 간선 개수

graph = [[INF] * (n+1) for _ in range(n+1)]
for a in range(1, n+1):
    graph[a][a] = 0 # 자기 자신노드에 대한 비용은 0으로 초기화

# 간선 정보
for _ in range(m):
    source, dest, cost = map(int, f.readline().split())
    graph[source][dest] = cost

floyd(graph, n)
print(graph)
```
{{</expand>}}

## 🤔 다익스트라와 비교
> 다익스트라를 모든 정점에서 돌려도 되지 않나?!

그럴 수 있지만...`플로이드-와샬`을 괜히 쓰는것은 아니다. 간선이 많지 않은 경우(`sparse graph`) 간선의 개수는 노드의 개수와 비슷해 $E \fallingdotseq V$ 이면 기존의 `다익스트라` 시간복잡도 $O(V^3)$로 플로이드-와샬과 같은 시간복잡도이다. 하지만 간선이 빽빽한 경우(`dense graph`) 모든 노드끼리 간선이 존재한다면 $E = \frac {v(v-1)}2 \fallingdotseq$ $V^2$로 `다익스트라` 시간복잡도 $O(VElogV)$이기 때문에 $O(V^3logV)$라면 `플로이드-와샬`보다 좋지 않은 시간복잡도이다. 또한 같은 시간복잡도여도 `플로이드-와샬`이 구현 난이도가 더 쉽다. 따라서 그래프 모든 쌍에서의 최단거리를 구하는 문제에서는 `플로이드-와샬`을 사용하는 것이 좋을 수 있다.