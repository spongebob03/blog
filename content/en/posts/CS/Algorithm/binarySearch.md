---
title: "BinarySearch"
date: 2022-04-20T17:48:51+09:00
draft: false
description: binary search, lower bound, upper bound
---
## 이진 탐색
{{<boxmd>}}
- 타겟이 중간값보다 **크면** 왼쪽 버리고 오른쪽만 보면 됨🔎 
- 타겟이 중간값보다 **작으면** 오른쪽 버리고 왼쪽만 보면 됨🔍
{{</boxmd>}}

{{< youtube B25Gu5r0xUg>}}

```python
def binarySearch(arr, target):
    left = 0
    right = len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid

        if arr[mid] > target:
            right = mid - 1
        else:
            left = mid + 1

    return -1
```
이진탐색은 기본적으로 정렬된 상태여야 합니다. 반복할수록 탐색할 범위가 절반으로 줄어들게 됨으로 시간복잡도는 O(logN)입니다.

## target의 "개수" 구하기
{{<boxmd>}}
🧦 `upper bound` - `lower bound` = `target의 개수`
{{</boxmd>}}
### Upper Bound
target <ins>초과</ins>인 값이 최초로 나오는 위치
```python 
def upperBound(arr, target):
    left = 0
    right = len(arr) - 1
    min_idx = len(arr)
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] > target:
            right = mid - 1
            min_idx = mid
        else:
            left = mid + 1

    return min_idx
```
### Lower Bound
target <ins>이상</ins>인 값이 최초로 나오는 위치
```python
def lowerBound(arr, target):
    left = 0
    right = len(arr) - 1
    min_idx = len(arr)
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] >= target:
            right = mid - 1
            min_idx = mid
        else:
            left = mid + 1

    return min_idx
```
