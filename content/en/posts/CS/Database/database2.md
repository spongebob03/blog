---
title: "DML, DCL, DDL"
date: 2022-04-23T21:44:43+09:00
draft: false
description: 데이터베이스-2
tags: ["데이터베이스", "CS"]
categories: ["데이터베이스"]
---
## DDL
데이터 정의어 Data Definition Language
데이터베이스나 **테이블** 등을 생성, 삭제, 변경을 위한 명령어

- 생성: `create table 테이블명;`
- 변경: `alter table 테이블명`
    - `add 속성 타입;`
    - `modify 속성 타입 (제약조건);`
    - `drop column 속성;`
    - `add primary key 속성;`
- 삭제: `drop table 테이블명;`

## DML 
데이터 조작어 Data Manipulation Langauge
데이터베이스에 저장된 데이터를 처리하거나 조회, 검색하기 위한 명령어. **튜플**에 대한 처리

- 조회: `select 속성 from 테이블;`
- 삽입: `insert into 테이블명 (속성1, 속성2..) values (값1, 값2..);`
- 수정: `update 테이블 set 속성1=값1, 속성2=값... where 조건;`
- 삭제: `delete from 테이블 where 조건;`

## DCL
데이터 제어어 Data Control Language
데이터 사용권한 관리. 데이터베이스에 저장된 데이터를 관리하기 위하여 데이터의 보안성 및 무결성 등을 제어하기 위한 명령어

- `grant` 권한 `on` 객체 `to` 사용자 `(with grant option);`
- `revoke` 권한 `on` 객체 `from` 사용자;