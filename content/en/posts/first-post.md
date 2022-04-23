---
title: "Hugo로 개발 블로그 장만하기"
date: 2021-12-02T22:09:48+09:00
draft: false
tags: ["blog", "hugo", "개발환경"]
series: ["우당탕탕 개발기"]
description: 야 너두 블로그 만들 수 있어
order: 1
---

![](https://blog.chodaeho.com/images/blog/2021/01/hugo-logo.png)  
SSG(Static site generator) 중 Hugo를 사용하여 블로그를 만들었다. 처음에 뭐가 뭔지 몰라서 뚱땅뚱땅 만들었다. 호스팅까지 하는데 하루 걸렸고 더 빠르게 할 수 있었지만 역시 세팅에서 시행착오를 겪었다. 그나마 Hugo여서 하루만에 할 수 있었던걸수도... Hugo는 Go로 만들어져서 나중에 Go에 대해서 더 공부해보고 싶다.

## github.io 만들기 
### Hugo 설치하기
`brew install hugo`  
`hugo version`
### 사이트 폴더 만들기
`hugo new site blog`
### 테마 붙이기
태그, 카테고리 잘보이는 테마가 안보여서 그나마 stack 테마가 제일 내가 원하는 템플릿이었다.
```
cd blog
git init
git submodule add https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack
```
`config.toml`파일 삭제  
### Github 저장소 연결
github.io 블로그를 호스팅하기 위해서는 깃헙 저장소 2개가 필요하다.
- `blog`: 블로그 컨텐츠
- `깃헙id.github.io`: 빌드된 웹사이트

```
git remote add origin https://blog_저장소_주소
git submodule add -b main git@github.io_저장소_SSH주소 public ## SSH키 있어야함
```
### 블로그 사용하기
themes/exampleSite의 `content` 폴더 내용을 복사해와서 사용. 
- 포스트 생성
  `hugo new post/포스트제목.md` 제목은 title 항목으로 고칠 수 있다.
### 댓글 기능 추가
`config.yaml`의 comments 설정만으로 간단하게 추가할 수 있다.    
깃헙계정으로 댓글을 남길 수 있는 [utterances](https://utteranc.es/)로 설정했다.  
```
comments:
        enabled: true
        provider: utterances

        utterances:
            repo: 깃헙계정/깃헙계정.github.io
            issueTerm: pathname
            label: comment
```
### 로컬 서버 확인
`hugo server (-D)` 웹서버 실행해서 수정사항 확인
### host on github
[공식 문서](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
1. `cd blog`
2. `hugo -t 테마`
3. `cd public`
4. `git add .`
5. `git commit -m "커밋메시지"`
6. `git push origin main` -> 내계정.github.io에 푸시(main이 기본 브랜치명이었다)  
7. `cd ..`
8. `git add .`
9. `git commit -m "커밋메시지"`
10. `git push origin master` -> blog 저장소에 푸시  

쉘 스크립트로 업로드 자동화  
  1. `deploy.sh`에 위에 매번 쓰는 커맨드를 정리  
  2. `./deploy.sh "커밋 메시지"`  
이후에는 Gist, GithubAction... 더 편하게 할 수 있는 방법 도전
## 추가할 사항  
- [x] utterances로 댓글기능 추가  
- [ ] 카테고리 사이드메뉴에 추가
- [ ] GithubAction으로 자동화
- [ ] hugo 기능 잘 활용하기
- [ ] 통계기능 추가
- [ ] custom domain 
  - https://www.holaxprogramming.com/2017/05/15/github-page-and-custom-domain/
  - https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages

## 참고 자료
[Hugo 공식문서 Quick Start](https://gohugo.io/getting-started/quick-start/#step-3-add-a-theme)  
[Hugo로 Github.io 블로그 만들기](https://github.com/Integerous/Integerous.github.io)
[Hugo theme Stack doc](https://docs.stack.jimmycai.com/)  
[초보자 Hugo 블로그 구축기](https://key4920.github.io/p/%EC%B4%88%EB%B3%B4%EC%9E%90-hugo-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EA%B5%AC%EC%B6%95%EA%B8%B0/)