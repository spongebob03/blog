---
title: "데이터베이스 정규화"
date: 2022-05-12T17:48:51+09:00
draft: false
description: 릴레이션을 분해하여 이상현상을 제거하는 정규화
tags: ["데이터베이스", "CS"]
categories: ["데이터베이스"]
libraries:
- mathjax
---
## 정규화 normalization
{{<boxmd>}}
이상현상이 발생하는 릴레이션을 분해하여 이를 없애는 과정.
이상현상을 일으키는 함수 종속성의 유형에 따라 등급을 구분할 수 있다.
{{</boxmd>}}

### 1NF: 제 1 정규형
{{<boxmd>}}
A relation in which the intersection of each row and column contains one and only one value.
릴레이션의 모든 속성 값이 원자값을 가지면 제 1 정규형이라고 한다. 
{{</boxmd>}}
#### 😈 이상현상
#### 🗝 이상현상 원인
#### 🛠 정규형 변환

### 제 2 정규형
{{<boxmd>}}
A relation that is in first normal form and every non-primary key attribute is fully functionally dependent on the primary key.
릴레이션이 제 1 정규형이고 기본키가 아닌 속성이 가본키에 완전 함수 종속일 때 제 2 정규형이라고 한다.
{{</boxmd>}}
#### 완전 함수 종속?
A와 B가 릴레이션의 속성이고 A -> B 종속성이 성립할 때, B가 A의 속성 전체에 함수 종속하고 부분 집합 속성에 함수 종속하지 않을 경우 완전 함수 종속이라고 한다. 
`(학번, 강좌이름)` -> `학점`
`학번` -x-> `학점` and `강좌이름` -x-> `학점`

#### 😈 이상현상
#### 🗝 이상현상 원인
#### 🛠 정규형 변환

### 제 3 정규형
{{<boxmd>}}
A relation that is in first and second normal form and in which no non-primary key is transitively dependent on the primary key.
{{</boxmd>}}
#### 😈 이상현상
#### 🗝 이상현상 원인
#### 🛠 정규형 변환

### BCNF
Boyce Codd Normal Form
{{<boxmd>}}
A relation is BCNF if and only if every determinant is a cadidate key.
{{</boxmd>}}
#### 😈 이상현상
#### 🗝 이상현상 원인
#### 🛠 정규형 변환