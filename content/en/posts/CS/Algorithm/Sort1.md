---
title: "정렬-1"
date: 2022-04-07T12:09:48+09:00
draft: false
tags: ["알고리즘", "정렬"]
categories: ["알고리즘"]
description: O(N^2) 시간 복잡도를 갖는 버블 정렬, 선택 정렬, 삽입 정렬, O(k*N)의 기수정렬
---
## Bubble Sort
{{< youtube xli_FI7CuzA>}}
{{< expand "구현 코드">}}
```python
def bubble_sort(arr):
    n = len(arr)

    for i in range(n-1):
        for j in range(n-1-i):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
    return arr
```
{{< /expand >}}

## Selection Sort
{{< youtube g-PGLbMth_g>}}
{{< expand "구현 코드">}}
```python
def selection_sort(arr):
    n = len(arr)

    for i in range(n-1):
        min = i # i번째로 작은 원소의 인덱스 일단 i로 초기화
        for j in range(i+1, n):
            if arr[min] > arr[j]:
                min = j
        arr[i], arr[min] = arr[min], arr[i]
    return arr
```
{{< /expand >}}

## Insert Sort
{{< youtube JU767SDMDvA>}}
{{< expand "구현 코드">}}
```python
def insert_sort(arr):
    n = len(arr)

    for i in range(n):
        j = i - 1
        key = arr[i]
        while j >= 0 and arr[j] > key:
            arr[j+1] = arr[j] # j를 오른쪽으로 한 칸 밀어줌
            j -= 1

        arr[j+1] = key
        
    return arr
```
{{< /expand >}}

## Radix Sort

{{< expand "구현 코드">}}
```python
def radix_sort(arr, k):
    for pos in range(k): # 10**pos 자리수
        arr_new = [[] for _ in range(10)]
        for i in range(len(arr)):
            digit = pos_digit(arr[i], pos)
            arr_new[digit].append(arr[i])

        store_arr = []
        for i in range(10):
            for j in range(len(arr_new[i])):
                store_arr.append(arr_new[i][j])
        arr = store_arr

    return arr

# 자리수 구하기
def pos_digit(number, pos):
    return (number % (10 ** (pos+1))) // (10 ** pos)
```
{{< /expand >}}