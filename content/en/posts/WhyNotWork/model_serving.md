---
title: "파이토치 모델 불러오기"
date: 2021-08-31T22:43:56+09:00
draft: false
series: ["이게 왜 안되지"]
description: 모델을 만들었는데 왜 쓰질 못하냔 말이야...

---

> _플레이리스트에 추천 곡을 채워주는 서비스를 작업중_

모델파트에서 모델파일이랑 필요한 부분을 주셔서 장고 웹에 붙이기만 하면되는데...생각보다 빨리 진행되지 않았다. 그 이유는...

## ModuleNotFoundError: No module named 'Utils'
[Pytorch torch.load -> ModuleNotFoundError: No module named 'utils']
(https://stackoverflow.com/questions/65538179/pytorch-torch-load-modulenotfounderror-no-module-named-utils)

Utils로 된 폴더 명이 겹쳐서 그런줄 알고 바꿨는데도...Utils라고 계속 뜨길래 대체 뭐지? 했는데 
torch.save했을때 쓴 Util 모듈과 같은게 있어야 하는거 같다. 

### 1.Utils 폴더에 필요한 파일 생성
### 2. khaiii 설치
[khaiii 깃허브](https://github.com/kakao/khaiii/wiki)
[khaiii 형태소 분석기에 설치 방법](https://pushnewsdev.github.io/2020/07/31/khaiii-install.html)
프로젝트 배포할때는 도커 써야겠다...

### gensim 설치
`pip install gensim`

## 이제 돌릴 수 있나?!
module notfound는 이제 없지만...
```
RuntimeError: Attempting to deserialize object on a CUDA device but torch.cuda.is_available() is False. If you are running on a CPU-only machine, please use torch.load with map_location=torch.device('cpu') to map your storages to the CPU.

[31/Aug/2021 13:12:03] "POST /playlist/recommend HTTP/1.1" 500 132085
```
ㅎㅎ...
### 추가 실행 오류
디바이스 cpu로 설정하고 했을때 보이는 오류들...cuda가 답인가
- [tqdm: 'module' object is not callable](https://stackoverflow.com/questions/39323182/tqdm-module-object-is-not-callable)
- KeyError: 4
- TypeError: dropout(): argument 'input' (position 1) must be Tensor, not str
	`pip install transformer==3`
- 불러올 파일 빠트리기ㅎ...팀원분의 도움으로 금방 찾을 수 있었다. 진작 물어볼걸!
- [Error: Expected more than 1 value per channel when training](https://discuss.pytorch.org/t/error-expected-more-than-1-value-per-channel-when-training/26274)
	- `model.eval()`로 해결할 수 있다. 
## 드디어 실행 성공
온갖 에러를 다봤다...금방 끝날 줄 알았지ㅎ...