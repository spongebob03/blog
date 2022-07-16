---
title: "Git 기본 활용"
date: 2022-04-01T14:36:02+09:00
draft: false
tags: ["git", "오픈소스"]
description: 프로젝트 관리, 오픈소스를 위한 간단한 깃 활용 연습하기! 버전 업데이트, 커밋, 브랜치까지
---
[공식문서](https://git-scm.com/doc)  
[누구나 쉽게 이해할 수 있는 Git 입문](https://backlog.com/git-tutorial/kr/intro/intro1_3.html)

## git 개념
### 버전 관리?
파일의 변화를 시간에 따라 기록했다가 필요한 경우 특정 시점의 버전을 다시 꺼내올 수 있는 시스템
- 중앙집중식 버전 관리(CVCS)
  - 중앙 서버에 문제가 생기면 협업 불가
- **분산 버전 관리 시스템**(DVCS)
  - 로컬 저장소가 원격 저장소의 파일, 히스토리 모두 복제 -> 백업 유리

## git 기본
### Repository: local - remote

- 원격 저장소 연결
    - `git clone <원격서버주소>` :원격 저장소 받아오기
    - `git remote add origin 원격저장소주소`
- upstream 저장소 설정
    - `git remote add upstream 저장소` : 포크한 저장소의 원본 저장소 연결
    - `git remote set-url **upstream** <오픈소스 저장소 주소>`
- 원격 저장소 확인
    - `git remote -v` : 원격 저장소 위치 확인
- 원격 저장소 연결 삭제
    - `git remote remove origin` : 원격 저장소 연결 끊기
- 원격 저장소 수정
    - `git remote set-url **origin** <변경할 원격서버 주소>` : 저장소 변경
    - `git remote update` → 원격 브랜치 갱신된다 → 수정 후 `commit` → 코맨트 남기고 → 확인 → 머지
- 메인 브랜치를 master가 아닌 main으로 설정하기
    
    `git branch -M main`

### Update
- upstream 저장소의 변경사항 받아오기
    1. `git fetch upstream`
    2. `git merge upstream/master`
    3. `git push origin master` : 로컬에서 원본 저장소의 내용으로 업데이트하고 이를 원격 저장소에 반영

### Commit
- 커밋 상태 조회
    - `git log --oneline`
    - `git log --branches --not --remotes` : ✅push하지 않은 커밋 확인✅
- 원격에 이미 올라간 커밋을 바꾸고 싶다면
    - 커밋 취소하기: `git reset --soft HEAD^`
    - 커밋 되돌리기: `git revert HEAD^` -> 커밋을 되돌린 것이 기록됨
      - `HEAD^`: 바로 이전 커밋
      - `HEAD~n`: n번째 전 커밋 (HEAD~1==HEAD^)
    - 커밋 덮어쓰기: `git commit --amend` 

### Branch

- 생성`git branch 브랜치이름`
- 이동
🧳 `checkout`: `.git` 에서 파일을 불러온다는 개념으로 생각
    - `git checkout 브랜치`
    - `git checkout -b 브랜치` : 브랜치 생성과 이동
- 확인 `git branch`
- 삭제
    
    main 브랜치로 이동 후
    
    - `git branch -D 브랜치`
    - `git push origin --delete 원격브랜치` : 원격 브랜치 삭제
- 브랜치 합치기
- 머지
    
    `git merge 합칠브랜치`

## Git Flow
[A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)   
[번역](http://dogfeet.github.io/articles/2011/a-successful-git-branching-model.html)  
[gitflow 구현 저장소](https://github.com/nvie/gitflow)   
[우아한 형제들의 적용 사례](https://techblog.woowahan.com/2553/)
