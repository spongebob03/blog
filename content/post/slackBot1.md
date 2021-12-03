---
title: "SlackBot으로 랜덤 문제 출제"
date: 2021-08-08T22:09:48+09:00
draft: false
tags: ["chatbot"]
categories: 
description: 
---
 스터디에서 4주 동안 배운 알고리즘 유형 문제를 정해진 시간에 풀어보는 모의 코딩테스트를 진행하고 싶었다. 백준 문제를 배운 알고리즘 내에서 랜덤으로 출제하는 슬랙 챗봇을 사용하면 재밌을거 같았다. [백준 사이트는 웹 스크래핑이 금지된다고 공지](https://www.acmicpc.net/board/view/2308)가 되어있어 solved.ac의 알고리즘 유형 카테고리 정보를 긁어오는 식으로 문제를 뽑아왔다. 혹시 이것도 문제가 되는지 문의해봐야겠다...
#### 사용 라이브러리
- BeautifulSoup
- slack_sdk
### 데이터 긁어오기
![](https://images.velog.io/images/spongebob03/post/3a536885-5dc4-4497-8213-a2590ac989ad/getData.png)
### 출제 유형 랜덤 선택, 문제 랜덤 선택
tag의 랜덤 범위가 5인 이유는 5개 알고리즘을 배워서...
![](https://images.velog.io/images/spongebob03/post/77194cbc-754d-456d-9afc-669ac3d3c9ea/random.png)
### 슬랙에 텍스트 보내기
[Slack api](https://api.slack.com/) > create app 
[Sending messages 문서](https://api.slack.com/messaging/sending)
원래 slacker 모듈을 사용했는데 적용이 안되어서 찾아보니 21년부터 slacker는 지원하지 않고 slack_sdk 패키지를 사용하는 것 같다. 
![](https://images.velog.io/images/spongebob03/post/f274ad9c-4d72-475d-9f35-d3c2a9b4fd8a/sendToSlack.png)

### 문제 상황 🤔
백준 사이트는 지나치게 많은 트래픽을 발생시키는 경우에는 사이트 이용이 정지된다고 합니다. 백준 사이트가 아닌 solved.ac의 알고리즘별 문제 목록을 긁어오는 것도 그래서인지 여러번 실행하면 아무 정보가 들어오지 않는다. 하지만 solvedac인데 왜..?

### 개선해야할 점
지금 상태는 파이썬 코드를 직접 실행시켜야 슬랙 챗봇이 실행된다. 파이썬 메인함수에서 스터디 기간동안 배운 알고리즘 유형들을 solved.ac에 있는 tag이름으로 찾아서 써줘야한다. 외부에서 간단하게 입력할 수 있었으면 좋겠다. 또한 다른 슬랙 앱처럼 슬랙에서 바로 실행시킬 수 있도록 할 필요가 있다.

 #### [참고 사이트]
 https://yganalyst.github.io/web/slackbot1/
 https://corikachu.github.io/articles/python/python-slack-bot-slacker
https://github.com/os/slacker

https://api.slack.com/authentication/oauth-v2
https://developerdk.tistory.com/96 슬랙커가 안먹히는 이유
https://api.slack.com/methods/chat.postMessage/code
https://slack.dev/python-slack-sdk/v3-migration/index.html#from-slackclient-2-x
https://pypi.org/project/slack-sdk/

https://pythonrepo.com/repo/slackapi-python-slack-sdk-python-third-party-apis-wrappers

https://github.com/slackapi/python-slack-sdk/issues/561