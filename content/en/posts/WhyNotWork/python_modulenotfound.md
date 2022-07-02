---
title: "ModuleNotFoundError: No module named 모듈"
date: 2021-08-04T22:43:56+09:00
draft: false
series: ["이게 왜 안되지"]
---

여러 파이썬 파일로 모듈을 구성하고 이를 연결할때 `ModuleNotFoundError: No module named 모듈` 에러를 확인하면 당황스러울 수 있다. 왜냐하면 이전까지 아무 생각 안하고 썼을때는 문제가 없었는데!

 처음에는 vscode 환경에서 경로를 따로 설정해줘야 하는지 찾아보았다. 
 두 번째는 가상환경에서 파이썬 경로가 다른건가 생각했다. 
 
 다른 프로젝트를 열어서 스크립트 두개를 만들어 확인해보니 문제가 없었다. 다시 작업하던 프로젝트로 돌아와 같은 테스트를 해보니 잘 돌아갔다! 여기서 알게 된 것은 같은 폴더였다는 것이다. 불러올 모듈이 다른 경로에 있으면 from 모듈로 찾을 수 없던 것이다. 
 
참고
[파이썬이 import한 모듈과 패키지를 찾는 과정](https://velog.io/@anrun/python-%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%9D%B4-import%ED%95%9C-%EB%AA%A8%EB%93%88%EA%B3%BC-%ED%8C%A8%ED%82%A4%EC%A7%80%EB%A5%BC-%EC%B0%BE%EB%8A%94-%EA%B3%BC%EC%A0%95)
[파이썬 상위폴더, 하위폴더에 있늠 모듈 불러오기](https://weejw.tistory.com/40)

다른 경로에 있는 모듈을 불러오고 싶다면 /는 사용하지 않고 .으로 하위 경로를 표시할 수 있다
`from 폴더.모듈 import 메서드`
예)
`from dataLoad.arena_util import json_load`