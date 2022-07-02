---
title: "AWS linux에 도커 설치"
date: 2021-09-10T22:43:56+09:00
draft: false
series: ["이게 왜 안되지"]
description: AWS 도커 설치 중 도커 클라이언트는 뜨는데 서버가 보이지 않는경우

---

[AWS 도커 설치](https://docs.aws.amazon.com/ko_kr/AmazonECS/latest/developerguide/docker-basics.html) 이대로 했는데
## 도커 서버가 보이지 않다면
[Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?](https://forums.docker.com/t/failure-to-start-docker-on-an-amazon-linux-machine/44003/15)
## Fix
1. `sudo service docker status`: 도커가 inactivate 상태인지 확인
2. `sudo service docker start`
3. `sudo systemctl enable docker`: 리부팅때 자동 실행되도록 설정
4. `sudo usermod -aG docker ec2-user`
5. `sudo reboot`