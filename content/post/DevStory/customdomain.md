---
title: "Hugo Blog with Custom Domain"
date: 2021-12-06T19:09:48+09:00
draft: false
tags: ["blog", "hugo", "개발환경"]
categories: ["우당탕탕 개발기"]
description: dns주소 설정하기
---

github.io 주소도 좋지만 간단하고 나만의 도메인 주소를 만들어서 쓰고 싶었다. 
## 도메인 구매
구매 사이트 비교(.dev 도메인 비용으로 찾아봤을 때 1년 비용. 기간에 따라 더 비싸지는 곳도 있다고 한다. 구매 전 기간을 설정하고 확인하자.)  
_원화 가격은 편의를 위해 만단위로 표시_
|사이트|1년 가격|
|:-:|:-:|
|google domain|$12|
|namecheap|$14.98|
|gandi|€14.01|
|godaddy|2,3890₩|
|gabia|2,9000₩|

 이 중 google이니 ad, analytics 사용할 때도 편하지 않을까 하는 기대로 google domain으로 정했다. 그런데 한국은 아직 서비스 하는 지역이 아니어서 결제를 못한다는..? 안내가 있어서 포기했다. 그러다가 역시 구글링을 통해 [지역을 바꾸고 진행할 수 있다는 글](https://starton.tistory.com/598)을 따라 무사히 결제하고 도메인을 받을 수 있었다. 
## 네임서버 레코드 설정
|호스트 이름|유형|TTL|데이터|
|:-:|:-:|:-:|:-:|
|커스텀도메인|A|기본|`185.199.108.153`|
|www.커스텀도메인|CNAME|기본|`깃헙계정.github.io`|

## GitHub 저장소에서 도메인 설정
`github.io 저장소` > `Settings` > `Pages` > `Custom domain`
1. 구입한 도메인 입력
2. Save
3. 조금 기다린 후 Enforce HTTPS 체크  
github.io 저장소에 CNAME 파일이 생성된다. 
## 🎉 완성
이제 내가 설정한 도메인을 이용할 수 있다! 
## ⚠️ 이후 빌드 커밋하기 전
github.io저장소에 CNAME파일이 생성되었기 때문에 그냥 빌드하고 커밋, 푸시할 때 에러날 수 있다. submodule로 연결되어 있는 public폴더를 미리 업데이트 해주면 문제 없다. 그전에 서브모듈 폴더(public, theme) 내부를 바꾸지 않았어야 한다. 
1. `cd blog`
2. `git pull`: main 프로젝트 pull
3. `git submodule update --remote --merge` : 연결된 서브모듈들의 업데이트 한번에 받아오기   
`git submodule update --remote <REMOTE-REPO-NAME> --merge`: 특정 서브모듈 업데이트만 받아오기
## 참고자료
Doc  
- [Hugo doc - Use a Custom Domain](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
[About custom domains and GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages)  
- [Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#about-custom-domain-configuration)

Blog
- [블로그 만들기 GitHub 심화 3편 - 커스텀 도메인](https://blog.chulgil.me/how-to-make-blog-using-github-3/ )  
- [블로그 커스텀 도메인 등록하기](https://devinlife.com/howto%20github%20pages/custom-domain/)  
- [Git submodule 사용하기](https://pinedance.github.io/blog/2019/05/28/Git-Submodule)