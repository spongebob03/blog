---
title: "Django runserver 에러"
date: 2021-08-23T22:43:56+09:00
draft: false
series: ["이게 왜 안되지"]
---


## When?
파이토치 모델 서빙하기 위해 예시를 보다가 [이미지 분류 장고 웹](https://github.com/atharva-18/Object-Detection-API)을 볼 수 있었다. 로컬에서 돌러보고싶어서 클론하고 requirements.txt로 필요한 모듈들을 다운받았다. 하지만 django manage.py runserver후 모듈관련 에러메시지가 보였다. 
`django.core.exceptions.ImproperlyConfigured: WSGI application 'application' could not be loaded;`
## Fix
1. pip install django-cors-headers
2. [pip install whitenoise](https://integer-ji.tistory.com/240)

## 참고
[django.core.exceptions.ImproperlyConfigured: WSGI application 'application' could not be loaded](https://stackoverflow.com/questions/41441832/django-core-exceptions-improperlyconfigured-wsgi-application-application-coul)