---
title: "데이터베이스-1"
date: 2022-04-23T17:48:51+09:00
draft: false
description: 데이터베이스 개념, 데이터 무결성
tags: ["데이터베이스", "CS"]
categories: ["데이터베이스"]
---
## 데이터베이스
> 프로젝트에 필요한 정보를 얻기 위해 논리적으로 연관된 데이터를 모아 구조적으로 통합해 놓은 것 - 데이터베이스 개론
### 개념

- **integrated data**: 중복을 최소화함으로써 데이터 불일치 현상을 없앤다
- **stored data**: 컴퓨터 저장장치에 저장된 데이터
- **operationl data**: 프로젝트의 목적을 위해 사용되는 데이터
- **shared data**: 여러 사람이 동시에 사용할 수 있다

### 특징

- **real time accessibility**: 사용자가 요청하는 순간에 실제 데이터를 서비스
    
- **continuous change**: 삽입, 삭제, 수정등으로 바뀐 데이터값 저장
    
- **concurrent sharing**: 동시에 여러 사용자가 데이터 요청가능
    
- **reference by content** 

### 데이터 구조

- 외부 단계- **외부 스키마**: 개념 스키마 중 사용자에게 필요한 부분
- 개념 단계- **개념 스키마**: 전체 데이터베이스의 정의. 통합 조직별로 하나만 존재.
    - 데이터 관계, 제약사항, 무결성
- 내부 단계- **내부 스키마**: 물리적 저장 장치에서 데이터베이스가 실제로 저장되는 방법의 표현.
    - 인덱스, 데이터 레코드의 배치 방법, 데이터 압축...

{{<boxmd>}}
💡 스키마: 데이터베이스의 조직이나 구조
{{</boxmd>}}

## 데이터 모델

![데이터테이블](/images/CS/db1.png)

[SQL 실습 사이트]([https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all](https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all))

## 데이터 무결성

데이터베이스에 저장된 데이터의 **일관성**과 **정확성**을 지키는 것.

### 무결성 제약조건

- **도메인** 무결성 제약조건
- **개체** 무결성 제약 조건(=기본키 제약)
    - NULL ❌
    - 키값 변동 ❌
    - 최소 집합. 최대한 적은 수의 속성만 가짐
    - 문제 발생 소지 ❌
- **참조** 무결성 제약 조건(=외래키 제약)

### key
{{<boxmd>}}
🔑 튜플을 유일하게 식별하는 장치, 각 릴레이션 간의 관계 말해주는 연결고리
{{</boxmd>}}
- 슈퍼키 superkey: tuple 유일하게 식별할 수 있는 속성 or 속성집합
- 후보키 candidate key: 튜플 유일하게 식별할 수 있는 최소집합
- 기본키 primary key: 후보키 중 하나 선정하여 대표로 삼은 키

![키 관계](/images/CS/db2.png)

- 대리키 surrogate key: 마땅한 키 없을 때 가상 속성 만들어 기본키로.
- 대체키 alternate key: 기본키로 선정되지 않은 후보키
- 외래키 foreign key: 다른 테이블 기본키를 참조하는 속성. 테이블 간 관계 표현
    
    ![외래키 예시](/images/CS/db3.png)
