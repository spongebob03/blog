---
title: "hugo 블로그 테마 바꾸기"
date: 2022-04-06T10:32:30+09:00
draft: false
tags: ["hugo", "blog"]
series: ["우당탕탕 개발기"]
description: "쓰고 싶은 테마가 생겼을때, 바꾸는 방법. 어때요, 참 쉽죠?"
image: images/thumnail/bob-ross.jpg
order: 1
---
휴고 블로그를 처음 만들었을때 나름 많이 찾아보고 고른 테마였는데도 뭔가 아쉬운 점이 많았다. 그래도 레이아웃도 커스텀하고 쓰고 있었는데 더 좋아보이는 테마를 발견해서 바꾸고 싶었다. 변경해야하는 파일이 많을까 걱정했지만 관련 블로그글을 따라해보니 생각보다 간단했다!

## 0. content파일 백업
작성한 글들이 있는 content폴더를 다른 곳에 복사해두고 3번 단계를 진행할때 변경된 구조에 필요한 부분을 복붙하는게 더 편할 수 있다. 물론 혹시 모르니 백업하는 건 중요하다...

## 1. 기존 테마 삭제
연결되어 있던 서브모듈 끊기
1. `git submodule deinit -f 기존테마`
2. `rm -rf .git/modules/기존테마`
3. `git rm -f 기존테마`
## 2. 새로운 테마 받기
새로운 서브모듈 연결
`git submodule add 새테마git저장소 themes/새테마`

## 3. 파일구조 맞추기


themes/테마/exampleSite의 파일구조를 참고하거나 복붙해서 기존의 작성한 글들을 알맞은 폴더로 이동시킨다. 테마에서 요구하는 기본 폴더들의 <ins>구조</ins>와 <ins>폴더명</ins>을 유지해야한다. 이후에 필수요소가 아니라면 자신이 원하는 블로그 형태에 맞게 삭제, 수정한다.

- 설정파일 수정하기
기존 테마는 `toml`파일 대신 `yaml` 설정 파일을 썼기 때문에 기존의 설정파일을 삭제한다. 보통 `themes/테마/exampleSite`에 있는 설정파일(`config.toml`)을 내 블로그 폴더 root 위치에 복사한다음 내 정보에 맞게 설정을 수정한다. 이번 테마는 설정파일들이 모두 config폴더 내에 있어서 config폴더 자체를 root 위치에 복붙해서 썼다. 

## 4. 로컬 서버에서 변경사항 확인하기
`hugo server`로 문제 없는지 확인
## 5. 블로그 저장소(blog, github.io) 업데이트
기존과 같지만 `hugo -t`부분을 새로운 테마명으로 하는 것을 주의한다. 원본저장소, public저장소에 올려서 변경사항을 반영한다. 
```git
cd blog
hugo -t 테마
cd public
git add .
git commit -m "커밋 메시지"
git push orgin main (내계정.github.io의 메인브랜치명 main이었다)
cd ..
git add .
git commit -m "커밋 메시지"
git push origin master (블로그 저장소 메인브랜치명은 master..)
```
## 🍀 참고 블로그
- [Git Submodule 삭제 방법](http://snowdeer.github.io/git/2018/08/01/how-to-remove-git-submodule/)
- [How to switch to another Hugo theme (Hugo 테마 변경하기)](https://hoontaeklee.github.io/en/posts/20200215_switch_theme/)