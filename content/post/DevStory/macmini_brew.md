---
title: "MacMini M1 에서 Homebrew 설치"
date: 2021-06-08T22:09:48+09:00
draft: false
tags: ["M1", "homebrew", "개발환경"]
categories: ["이게 왜 안되지"]
description: M1...
---
## Homebrew?
[그냥 사이트에서 다운받는거랑 뭐가 다른가요?](https://stackoverflow.com/questions/17265226/what-is-the-difference-between-installing-an-app-via-homebrew-or-installing-it)

macOS용 패키지 매니저로 프로그램 설치, 업데이트, 삭제를 편하게 관리할 수 있다. (하지만 윈도우만 쓰던 나는 나중에 알았다..)

### M1 네이티브로 설치하려 했으나..실패!
 그냥 뭔가 커맨드가 안먹혀서 의문이었는데 기존 MAC을 위한 안내는 기존 실리콘칩. 내 컴퓨터는 m1칩이라 그런 것이었다.
 
### 🛠해결
 M1 네이티브로 설치하는 방법도 있으나 이전 맥 터미널과 동일하게 호환할 수 있는 Rosetta를 사용하는 방법이 제일 간단했다.
1. 터미널앱 > 정보 가져오기> ☑️ **Rosetta를 사용하여 열기**
2. [Homebrew](https://brew.sh/index_ko) 설치
`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

참고  
[설치하기 이전에 이걸 봤어야 했는데...](https://www.youtube.com/watch?v=j-933jvH8sE)