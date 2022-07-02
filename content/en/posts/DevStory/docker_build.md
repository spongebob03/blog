---
title: "Django 프로젝트 도커 이미지 생성하기"
date: 2021-09-07T22:43:56+09:00
draft: false
tags: ["docker", "django"]
description: 도커 생각보다 안 어려워요~

---

> 추천시스템에 관한 프로젝트를 진행하면서 점점 커지는 프로젝트 내용과 설정들, github에 용량 커서 안올라가는 파일들도 있어서 배포할때 어떻게 해야할지 고민이었는데...
[로컬에서 도커 빌드하기](https://velog.io/@devmin/Docker-deployment)

## 0. gunicon 설치
## 0-1. pip freeze > requirements.txt
설치 필요한 모듈들 requirements.txt에 모두 쓰기 (도커파일 작성하기전에 확인하기. 버그 고치다가 쓸데없이 설치한 모듈들도 있어서 transformers, tokenizers...)
그래도 이런 경우가 생길 수 있으니 링크는 저장해둔다. 
[Failed building wheel for tokenizers](https://github.com/huggingface/transformers/issues/2831)
[error: Can not find Rust compiler](https://github.com/docker/compose/issues/8105)
[cryptography 오류시 대처법](https://doitdjango.com/blog/4/)
# 1. 도커파일 Dockerfile 작성하기
프로젝트 최상위 경로에 `Dockerfile` 생성하기.
vscode로 작업한다면 extention에서 docker를 설치하고 작성하면 가독성이 편하다.
```
FROM python:3.8-slim-buster
WORKDIR /usr/src/app

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED=1
ENV CRYPTOGRAPHY_DONT_BUILD_RUST=1

# Required pkgs installation
RUN pip install --upgrade pip
COPY requirements.txt ./
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
RUN pip install -r requirements.txt


# Port setting
EXPOSE 8000

# gunicorn execution
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "Web.wsgi:application"]
```

# 2. khaiii가 있었구나..!
도커파일 Port setting 전에 다음 내용 추가
```
#khaiii
RUN apt-get update && apt-get install -y git build-essential
RUN apt -y update
RUN apt -y install build-essential locales locales-all

WORKDIR /root
RUN git clone https://github.com/kakao/khaiii.git

WORKDIR /root/khaiii
RUN pip install -r requirements.txt

WORKDIR /root/khaiii/build
RUN cmake ..
RUN make all
RUN make resource
RUN make install

RUN make package_python
WORKDIR /root/khaiii/build/package_python
RUN pip install .

RUN locale-gen en_US.UTF-8
RUN update-locale LANG=en_US.UTF-8
```
khaiii에 기본으로 있던 도커파일을 그대로 쓰면 git부분이나 다른 부분에서 에러가 나서 도커 이미지 빌드가 안될 수 있다. 다음과 같이 필요한 부분을 더 추가한 내용으로 고쳐쓰니 무사히 빌드가 되었다. 
# 3. 도커파일 빌드
`docker build -t 계정/이미지:0.1.1 .`
### 도커 이미지 확인
`docker images`
# 4. 도커허브에 올리기
`docker push 이미지:버전`
### denied: requested access to the resource is denied
1. docker login
2. 이미지 이름에 도커허브 계정이 빠졌다면
[잘못된 이미지 이름 바꾸기](https://stackoverflow.com/questions/25211198/docker-how-to-change-repository-name-or-rename-image)

## 도커허브에서 이미지 받기
`docker pull 이미지:버전`
## 이미지 실행
`docker run -d -p 8000:8000 내계정/django_web:0.1.1`
### 실행 확인
`docker ps`
## 중지
`docker stop id일부`

# 삽질⛏
## COPY는 RUN처럼 &&을 사용할 수 없다
