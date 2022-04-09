---
title: "정렬2"
date: 2022-04-08T12:09:48+09:00
draft: false
tags: ["알고리즘", "정렬"]
categories: ["알고리즘"]
description: O(NlogN) 시간 복잡도를 갖는 병합 정렬, 퀵 정렬, 힙 정렬
---

## Merge Sort
{{< youtube QAyl79dCO_k >}}

{{< expand "구현 코드" >}}

```Python
def merge_sort(arr, left, right):
    if left < right:
        mid = (left + right) // 2
        merge_sort(arr, left, mid)
        merge_sort(arr, mid+1, right)
        merge(arr, left, mid, right)

def merge(arr, left, mid, right):
    i = left
    j = mid + 1

    merged_arr = [0] * (right+1)
    k = left
    while i <= mid and j <= right:
        if arr[i] <= arr[j]:
            merged_arr[k] = arr[i]
            k += 1
            i += 1
        else:
            merged_arr[k] = arr[j]
            k += 1
            j += 1

    while i <= mid:
        merged_arr[k] = arr[i]
        k += 1
        i += 1
    while j <= right:
        merged_arr[k] = arr[j]
        k += 1
        j += 1
    
    for k in range(left, right+1):
        arr[k] = merged_arr[k]
    
    return arr
```
{{</expand>}}

## Quick Sort
{{< youtube 7BDzle2n47c >}}

{{< expand "구현 코드">}}
다른 자료의 psuedo코드 기반으로 설명 영상의 s, e 동작과는 다르게 구현되었습니다.
```python
def quick_sort(arr, left, right):
    if left < right:
        pos = partition(arr, left, right)

        quick_sort(arr, left, pos - 1)
        quick_sort(arr, pos + 1, right)

def partition(arr, left, right):
    pivot = arr[right] # 가장 오른쪽 원소를 pivot으로 잡는 경우
    s = left - 1 # s는 pivot보다 작은 수를 가리킨다

    for e in range(left, right): # e는 pivot보다 같거나 큰 수를 가리키는데
        if arr[e] < pivot: # 그렇지 않으면 
            s += 1
            arr[s], arr[e] = arr[e], arr[s] # swap
            

    arr[s+1], arr[right] = arr[right], arr[s+1] # pivot 변경
    return i + 1 # pivot 최종 위치 반환
```
{{< /expand >}}

## Heap Sort

{{< youtube 2DmK_H7IdTo>}}

{{< expand "구현 코드">}}
```python
def heap_sort(arr, n):
    for i in range(n//2, 0, -1):
        build_heap(arr, n, i)
    
    for i in range(n, 1, -1):
        arr[1], arr[i] = arr[i], arr[1]
        build_heap(arr, i, 1)

# heapify method (max-heap)
def build_heap(arr, n, i):
    largest = i
    l = i * 2 # 왼쪽 자식 노드 인덱스
    r = i * 2 + 1 # 오른쪽 자식 노드 인덱스

    if l < n and arr[l] > arr[largest]:
        largest = l

    if r < n and arr[r] > arr[largest]:
        largest = r

    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        build_heap(arr, n, largest)

# 힙의 부모-자식 인덱스 관계를 위해서 1번 인덱스부터 루트노드가 들어간다
arr = [-, 6, 1, 4, 8, 3, 9, 2, 5]
heap_sort(arr, 8)
print(arr)
```
{{< /expand >}}