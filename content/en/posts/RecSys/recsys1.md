---
title: "추천시스템이란?"
date: 2021-11-09T22:09:48+09:00
draft: false
tags: ["추천시스템"]
categories: ["Recommendation System"]
---
> 사용자가 관심있어 할만한 아이템을 제공해주는 자동화된 시스템
- 핵심
  - 관심을 어떻게 표현하는가?
  - 관심을 어떻게 측정하는가?
=> 유사도 측정

## 사용자와 아이템
- 사용자: 서비스를 사용하는 사람
- 아이템: 서비스에서 판매하는 물품(다른 사용자가 물품이 될 수 있음)
보통 서비스가 성장하면 사용자, 아이템의 수도 같이 성장함
  - 특히 사용자의 성장도가 훨씬 커짐
- 하지만 아이템의 수가 커지면서 아이템 디스커버리 문제가 대두
    - 모든 사용자가 검색(능동적)을 하지 않으며 사람들이 점점 더 추천(수동적) 선호
## 필요성
- 조금의 노력으로 사용자가 관심있어 할만한 아이템을 찾아주는 방법
    - 아이템의 수가 굉장히 큰 경우 더 의미가 있다
    - 수작업으론...도저히 불가 → 자동화 필요!
    - 개인화(Personalization)으로 연결됨
    - 확증편향의 문제가 있을 수 있다
        - 가끔씩 전혀 관심없을 듯한 아이템도 추천 가능(Serendipity)
- 회사 관점에서는 추천 엔진을 기반으로 다양한 기능 추가 가능
    - 마케팅시 추천 엔진 사용(이메일 마케팅)
    - 관련 상품 추천으로 쉽게 확장 가능
- 아이템 수가 많아서 원하는 것을 찾기 쉽지 않은 경우
    ⇒ 검색을 위한 수고를 덜어줌
- 추천을 통해 신상품등의 마케팅이 가능해짐
    - 새로 나온 아이템들은 노출 자체가 어려운데 추천을 통해 기회를 줄 수 있다
- 인기 아이템 뿐만 아니라 롱 테일의 다양한 아이템 노출이 가능
    - 추천 방식에 따라 다르지만 개인화가 잘 되면 이게 가능해짐
## 추천은 매칭 문제!
- 사용자에게 맞는 아이템을 매칭해주기
    - 아이템은 서비스에 따라 달라지며 아이템이 다른 사용자가 될 수도 있음 (친구 추천, 강의 추천...)
- 어떤 아이뎀을 추천할 것인가?
    - 다양한 방법이 존재
        - 지금 뜨는 아이템 추천(개인화되어 있지 않음)
        - 사용자가 마지막에 클릭했던 아이템들 다시 추천
        - 사용자가 구매했던 아이템들을 구매한 다른 사용자들이 구매한 아이템 추천 ← *협업 필터링*
- 추천 UI도 굉장히 중요
    - 추천 유닛 존재. 일르 어떤 순서로 어떻게 노출하는지가 중요
    - 구체적인 이유를 보여주는 것이 좋다
- 사용자와 아이템에 대한 부가 정보들이 필요해짐
- 아이템 부가 정보
    - 분류 체계
    - 태그 형태로 부가정보를 유지하는 것도 아주 좋음
- 사용자 프로파일 정보
    - 개인정보: 성별, 연령대 → 콜드 스타트 문제 해결
    - 아이템 정보:
        - 관심 카테고리와 서브 카테고리(분류체계 기반), 태그
        - 클릭 혹은 구매 아이템
- 무엇을 기준으로 추천을 할 것인가?
## 일상에서 볼 수 있는 추천
- 아마존 관련 상품 추천
    키보드 샀다면 → 같이 쓸 마우스 추천
- 넷플릭스 영화, 드라마 추천
    이전에 봤던 작품 → 다른 드라마, 영화 추천
    `추천 타이틀`: 이 작품들 왜 추천하는지
- 구글 검색어 자동 완성
    편리, 내가 생각하지 못했던 정보로도 검색할 수 있어서
    개인정보 이슈가 있었다
- 링크드인 or 페이스북 친구 추천
  - 사용자: 멤버
  - 아이템: 멤버
  왜 추천하는지 이유를 보여준다
- 스포티파이 노래, 플레이리스트 추천
- 헬스케어 도메인의 위험 점수 계산
    사용자: 의사, 간호사
    아이템: 환자
    어느 환자를 먼저 치료하는게 좋은지 점수로 수치화
    

### → 공통점

- 격자 형태 UI 사용
- 다양한 종류의 추천 유닛 존재
    - 일부 유닛은 개인화
    - 일부 유닛은 인기도 등 비 개인화 정보기반
    - 추천 유닛의 랭킹 중요
        - 이 부분도 모델링하여 개인화하는 추세
        - 클릭을 최적화하고 이 데이터 수집을 위한 실험을 함
            - 순전히 데이터 수집을 위한 온라인 테스트