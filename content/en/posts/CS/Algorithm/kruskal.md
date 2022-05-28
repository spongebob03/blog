---
title: "크루스칼 알고리즘"
date: 2022-05-15T23:01:42+09:00
draft: false
description: Kruskal Algorithm
tags: ["알고리즘", "그래프", "최단경로"]
categories: ["알고리즘"]
libraries:
- mathjax
---
## MST
Spanning Tree: 최소한 간선을 사용하여 그래프 내 모든 정점을 이은 트리(트리는 사이클이 없다는거 주의)
n개의 정점에 n-1개의 간선이 존재하는 그래프는 트리
가중치가 있는 그래프에서 최소 비용을 사용하면 Minimum Spanning Tree
## Union-Find
특정 원소들을 합칠때 이들이 같은 집합에 포함되어 있는지 확인하거나 두 집합을 합칠때 사용한다. 

## Kruskal Algorithm
최소비용 **간선**을 선택해나간다. 이때 사이클이 발생하지 않도록 하는데 그 과정에서 `Union-Find`를 사용하면 사이클이 발생하는지 체크할 수 있다.

간선의 가중치가 작은 것부터 순서대로 보면서 해당 간선 양 끝에 있는 두 노드 x, y에 대해 find(x), find(y)값을 비교하여 일치하지 않는 경우에만 간선을 선택해주고 union(x, y)를 진행해주는 식으로 계속 진행한다.
- 시간복잡도: $O(ElogE)$

{{<youtube 71UQH7Pr9kU>}}

```python
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)

    if a < b:
        parent[b] = a
    else:
        parent[a] = b

def kruskal(edges):
    result = 0
    
    # 간선을 비용이 작은 것부터 정렬
    edges.sort()

    for edge in edges:
        cost, a, b = edge

        # 두 지점을 이었을때 사이클이 없으면
        if find_parent(parent, a) != find_parent(parent, b):
            union_parent(parent, a, b)
            result += cost
    
    return result

v, e = map(int, input().split()) # 정점, 간선 개수
parent = [i for i in range(v+1)] # 각 정점이 속한 그룹 초기화
edges = []
# 간선 정보 입력
for _ in range(e):
    a, b, cost = map(int, f.readline().split())
    edges.append((cost, a, b))
```