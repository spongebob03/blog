---
title: "Hugo로 개발 블로그 장만하기"
date: 2021-12-02T22:09:48+09:00
draft: false
tags: ["blog", "hugo"]
categories: ["우당탕탕 개발기"]
description: 야 너두 블로그 만들 수 있어
---

## github.io Blog with Hugo!
![Hugo-Static Site Generator](https://blog.chodaeho.com/images/blog/2021/01/hugo-logo.png)  
SSG 중 Hugo를 사용하여 블로그를 만들었다. 처음에 뭐가 뭔지 몰라서 뚱땅뚱땅 만들었다. 
이제 배포를 해야하는데 어떻게 하더라...

## github.io 만들기 
### Install Hugo
`brew install hugo`
`hugo version`
### 사이트 폴더 만들기
`hugo new site blog`
### 테마 붙이기
테그, 카테고리 잘보이는 테마가 안보여서 그나마 stack 테마가 제일 내가 원하는 템플릿이었다...
```
cd blog
git init
git submodule add https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack
```
`config.toml`파일 삭제  
### 템플릿 데모 사이트 참고해서 수정하기
`content` 폴더 내용을 복사해와서 사용. 
- 포스트 생성
  `hugo new post/포스트제목.md` 제목은 title 항목으로 고칠 수 있다.
### 확인
`hugo server (-D)` 웹서버 실행해서 수정사항 확인
### host on github
[공식 문서](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
## 참고 자료
[Hugo 공식문서 Quick Start](https://gohugo.io/getting-started/quick-start/#step-3-add-a-theme)  
[Hugo로 Github.io 블로그 만들기](https://github.com/Integerous/Integerous.github.io)
[Hugo theme Stack doc](https://docs.stack.jimmycai.com/)  
[초보자 Hugo 블로그 구축기](https://key4920.github.io/p/%EC%B4%88%EB%B3%B4%EC%9E%90-hugo-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EA%B5%AC%EC%B6%95%EA%B8%B0/)