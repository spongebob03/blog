---
title: "터미널 꾸미기"
date: 2021-07-23T22:09:48+09:00
draft: false
description: 밋밋했던 터미널 이젠 안녕~
---

## 1. iTerm2 설치
`brew install --cask iterm2`
## 2. Powerlevel10k 권장 폰트 설치
설치한 다음 iterm-> 폰트 설정에서 `MesloLGS NF` 선택
vscode 콘솔에서 프롬프트가 깨지는 문제가 있었는데 이부분도 폰트를 추가해줘야 해결된다

## 3. Powerlevel10k 설치
```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
```
원래 이거 전에 Oh-My-Zsh 설치인거 같은데 매뉴얼 따라하다가 테마 먼저 받은거 같다...
## 4. Oh-My-Zsh
```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```
## 5. 테마 설정
- `source ~/powerlevel10k/powerlevel10k.zsh-theme`: 초기설정
-  `p10k configure`: 실행하면 다시 설정할 수 있다

## 참고
[Powerlevel10k 공식](https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k)
[맥 터미널 환경 세팅](https://subicura.com/2017/11/22/mac-os-development-environment-setup.html)
[설치 순서](https://velog.io/@xedni/I-Term-%EB%82%B4-%ED%84%B0%EB%AF%B8%EB%84%90-%EC%98%88%EC%81%98%EA%B2%8C-%EA%BE%B8%EB%AF%B8%EA%B8%B0)
[Powerlevel10k로 zsh 설정하기-outsider](https://blog.outsider.ne.kr/1490)
