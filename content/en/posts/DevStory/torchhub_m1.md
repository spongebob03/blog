---
title: "파이토치 허브 M1 로컬에서 빌드하기"
date: 2021-08-16T22:09:48+09:00
draft: false
tags: ["M1", "오픈소스", "개발환경"]
series: ["이게 왜 안되지"]
description: 간단한 빌드 중 만나게 되는 에러들^^*
---
> [Github) PyTorchKR](https://github.com/9bow/PyTorchKR)

### nvm: command not found
[노드 버전관리 설치 및 사용법](https://kood-dev.tistory.com/3)
1. `find ~/.zshrc`로 /.zshrc파일 있는지 확인
2. `open ~/.zshrc` 파일 열어서 아래 내용 추가
```
export NVM_DIR=~/.nvm 
source $(brew --prefix nvm)/nvm.sh
```
4. `source ~/.zshrc` 코드 적용
3. nvm --version으로 확인


### rbenv: version `2.5.9' is not installed 
brew install rbenv했지만 [이걸 또 m1이..?]
(https://github.com/rbenv/ruby-build/issues/1691)
해결 방법은 간단했습니다. 
`rbenv install 2.5.9`

### ERROR: While executing gem...Gem::FilePermissionError

[Mac에서 Gem::FilePermissionError 에러 발생시 해결 방법](https://jojoldu.tistory.com/288)
~/.zshrc 파일에 아래 내용 추가
```
[[ -d ~/.rbenv  ]] && \
  export PATH=${HOME}/.rbenv/bin:${PATH} && \
  eval "$(rbenv init -)"
```
`source ~/.zshrc`로 코드 적용


### bundle install 에서 make: yarn: No such file or directory

`npm install -g yarn`
### 빌드
`make serve`
![](https://images.velog.io/images/spongebob03/post/2d0b56cb-0994-430f-9b91-593d8f6237a0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-08-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.15.55.png)
멘토님의 친절한 커맨드 예시와 오늘도 구글링으로 얻은 정보들에 감사하며...🥳