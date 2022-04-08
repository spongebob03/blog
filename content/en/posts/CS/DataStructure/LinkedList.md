---
title: "Linked List"
date: 2022-03-05T12:09:48+09:00
draft: false
tags: ["자료구조", "연결리스트"]
---

## 연결 리스트
연결된 데이터를 저장할때 사용합니다.  
연결리스트를 구성하는 노드의 구조는 다음과 같습니다.
{{<boxmd>}}
- node
    - data
    - link(next)
{{</boxmd>}}

|장점|단점|
|:-:|:-:|
|삭제와 삽입 O(1)<br/>길이가 정해지지 않은 데이터를 다룰때 유용|메모리 요구량이 더 크다<br/>  원하는 위치 검색할 때 처음부터 다 확인해야한다|

### 배열과 비교
||배열|연결리스트|
|:-:|:-:|:-:|
|삽입/삭제|O(n)|O(1)|
|저장공간|연속한 위치|임의의 위치|
|탐색|O(1)|O(n)|

## 단순 연결 리스트
### 구현
노드
```python
class Node:
    def __init__(self, item):
        self.data = item
        self.next = None
```
연결 리스트
    
```python
class LinkedList:
    def __init__(self):
        self.nodeCount = 0  # 노드의 개수
        self.head = None    # 처음 노드
        self.tail = None    # 끝 노드
```

{{< expand "탐색: k번째 원소 찾기">}}  
```python
def getAt(self, pos):
    if pos < 1 or pos > self.nodeCount: # 범위 벗어났다면
        return None
    i = 1
    curr = self.head # 맨 앞 노드부터
    while i < pos:
        curr = curr.next
        i += 1
    return curr
```
{{< /expand >}}

{{< expand "리스트 순회">}}
```python
def traverse(self):
    answer = []
    curr = self.head
    while curr is not None:
        answer.append(curr.data)
        curr = curr.next
    return answer
```
{{< /expand >}}

{{< expand "길이 알아내기">}}
```python
def __len__(self):
    return self.nodeCount
```
{{< /expand >}}

{{< expand "원소 삽입">}}
```python
def insertAt(self, pos, newNode):
    if pos < 1 or pos > self.nodeCount + 1: # 범위 밖 인덱스라면
        return False
    
    if pos == 1: # 맨 앞에 삽입
        newNode.next = self.head
        self.head = newNode
    else:
        if pos == self.nodeCount + 1: # 맨 뒤에 삽입
            prev = self.tail
        else: 
            prev = self.getAt(pos - 1) # 중간에 삽입
        newNode.next = prev.next
        prev.next = newNode
    
    if pos == self.nodeCount + 1:
        self.tail = newNode
    
    self.nodeCount += 1
    return True
```
{{< /expand >}}

{{< expand "원소 삭제" >}}
```python
def popAt(self, pos):
    if pos <= 0 or pos > self.nodeCount:
        raise IndexError

    if self.nodeCount == 1: # 노드가 하나밖에 없는 연결리스트
        curr = self.getAt(pos)
        self.head = None
        self.tail = None
        self.nodeCount -= 1
        return curr.data

    # 삭제하려는 노드가 맨 앞이면 -> prev 없음. Head 조정
    if pos == 1:
        curr = self.head
        self.head = curr.next
    # 리스트 맨 끝 노드 삭제할때 -> Tail 조정 필요
    elif pos == self.nodeCount:
        curr = self.tail
        prev = self.getAt(pos - 1)
        prev.next = None
        self.tail = prev
    else:
        prev = self.getAt(pos - 1)
        curr = prev.next
        prev.next = curr.next
    self.nodeCount -= 1
    return curr.data
```
{{< /expand >}}

{{< expand "두 리스트 합치기" >}}
```python
def concat(self, L):
    self.tail.next = L.head
    if L.tail:
        self.tail = L.tail
    self.nodeCount += L.nodeCount
```
{{< /expand >}}
## 원형 연결 리스트
## 이중 연결 리스트
## 참고자료

[🖥  연결리스트 연산 PPT](https://docs.google.com/presentation/d/1JRd56Gl5Z2z6wXwRUnuzugsTLbS_1IzRWE4i9vDg-MA/edit?usp=sharing)

[자료구조09 - 연결리스트(Linked List)2](https://blog.naver.com/PostView.naver?blogId=glgkwls1&logNo=222343882010&categoryNo=13&parentCategoryNo=0)

[https://wayhome25.github.io/cs/2017/04/17/cs-19/](https://wayhome25.github.io/cs/2017/04/17/cs-19/)

[https://opentutorials.org/module/1335/8821](https://opentutorials.org/module/1335/8821)