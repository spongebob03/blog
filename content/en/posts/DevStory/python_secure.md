---
title: "파이썬 시큐어코딩 가이드"
date: 2022-05-15T23:13:39+09:00
draft: false
libraries:
- mathjax
description: <Python 시큐어코딩 가이드> - KISA
tags: ["보안"]
---

## 입력데이터 검증 및 표현
### 1. SQL 삽입
{{<boxmd>}}
😈 데이터베이스와 연동된 웹 응용 프로그램에서 입력된 데이터에 대한 유효성 검증을 하지 않을 경우 공격자가 **입력 폼 및 입력란에 SQL문을 삽입하여 DB로부터 정보를 열람하거나 조작**할 수 있는 보안취약점
{{</boxmd>}}
Python에서는 데이터베이스에 엑세스에 사용되는 다양한 Python 모듈간의 일관성을 장려하기 위해 DB-API를 정의해서 각 데이터베이스마다 별도의 DB 모듈을 이용해 데이터베이스에 엑세스하게 된다. DB-API 외에도, 파이썬에서는 ORM을 사용하여 데이터베이스에 엑세스할 수 있다. 

ORM(Object RElation Mapping)을 이용하면 보다 <ins>안전하게 DB를 사용할 수 있지만 일부 복잡한 조건의 쿼리문 생성이 어렵고 성능저하</ins> 등의 이유로 쿼리의 튜닝이 필요한 경우 직접 원시 SQL 실행이 필요한 경우가 있다. ORM 대신 원시 쿼리를 사용하는 경우 검증되지 않은 외부 입력값으로 인해 SQL 삽입 공격이 발생할 수 있다.


#### 👮‍♀️ 시큐어 코딩
- DB-API, 원시SQL: 매개변수화된 쿼리 사용하여 외부 입력값을 바인딩해서 사용하기
- ORM(Django's querySets, SQLALchemy, Storm): 따로 필요없음. ORM 프레임워크는 내부적으로 사용되는 쿼리 모든 곳에서 매개변수화된 명령문을 사용하므로 기본으로 SQL삽입 공격으로부터 보호된다.

```python
from django.shortcuts import render

def update_board(request):
   ......
    with dbconn.cursor() as curs:
        name = request.POST.get('name', '')
        content_id = request.POST.get('content_id', '')

        # 😱 절망편: 사용자의 검증되지 않은 입력으로 부터 동적으로 쿼리문 생성
        sql_query = "update board set name='" + name + "' where content_id='“ + content_id + "'"
        # 😇 희망편: 외부 입력값으로 부터 안전한 매개변수 화된 쿼리를 생성 한다
        sql_query = 'update board set name= %s where content_id=%s'
        
        # 😱 외부 입력값이 검증 없이 쿼리로 수행되어 안전하지 않다
        curs.execute(sql_query)
        # 😇 사용자의 입력 값을 매개변수 화된 쿼리에 바인딩 하여 실행되므로 안전하다
        curs.execute(sql_query, (name, content_id))

        curs.commit()
        return render(request, '/success.html')
```

```python
from django.shortcuts import render     
from app.models import Member

def member_search(request):
    # 외부로부터 입력 값을 가져온다
    name = request.POST.get('name', '')     
    
    # 😱 외부로부터 입력 받은 값을 검증 없이 쿼리문 생성에 사용하여 안전하지 않다
    query=“select * from member where name=‘” + name + “’”
    # 😇 외부 입력 값을 raw()함수 실행 시 바인딩 변수로 사용하여 쿼리 구조가 변경되지 않도록 한다. (list 형은 %s, dictionary형은 %(key)s를 사용)
    query='select * from member where name=%s'

    # 😱 raw() 외부 입력 값을 검증 없이 사용한 쿼리문을 함수로 실행하면 안전하지 않다
    data = Member.objects.raw(query)
    # 😇 인자화된 쿼리문을 사용하여 raw()함수를 사용하여 안전
    data = Member.objects.raw(query, [name])

    return render(request, '/member_list.html', {'member_list':data})
```
### 2. 코드 삽입
{{<boxmd>}}
프로그래밍 언어 자체의 기능에 한해 이뤄지며 취약한 프로그램에서 **사용자의 입력 값에 코드가 포함되는 것을 허용할 경우**, 공격자가 임의 코드를 삽입해 소프트웨어를 비정상적으로 동작시켜 공격자가 의도한 동작을 하도록 만드는 보안약점. 개발자가 의도하지 않은 코드를 실행하여 권한을 탈취하거나 인증 우회, 시스템 명령어 실행 등을 할 수 있다. 
{{</boxmd>}}
Python에서 코드 삽입 공격을 유발할 수 있는 함수로 eval(), exec()등의 함수가 있다. 이 함수들을 사용할때 더욱 주의할것.
#### 👮‍♀️ 시큐어 코딩
- 동적코드를 실행할 수 있는 함수를 사용하지 않는다 or 실행 가능한 동적코드를 입력 값으로 받지 않도록하고 외부 입력값에 대하여 화이트리스트 방식으로 검증한다. 
- 유효한 문자만 포함하도록 동적 코드에 사용되는 사용자 입력 값을 필터링

1. eval() 함수 사용 예제
```python
from django.shortcuts import render

def route(request):
    message = request.POST.get('message', '')

    # 👿 eval함수에 외부 입력값을 검증 없이 사용할 경우 코드가 실행될 수 있어 위험
    ret = eval(message)
    return render(request, '/success.html', {'data':ret})
    # 😇 사용자 입력을 영문, 숫자로 제한. 특수문자가 포함되어 있을 경우 에러 메시지 리턴
    if message.isalnum():
        ret = eval(message)
        return render(request, '/success.html', {'data':ret})
```
예) Email 형식의 입력만 받아야할 경우 정규식으로 검증할 수 있다.
`prog = re.compile(r'([A-Za-z0-9]+[.-_])*[A-Za-z0-9]+@[A-Za-z0-9-]+(\.[A-Z|a-z]{2,})+')`

2. exec()함수 사용 예제
```python
from django.shortcuts import render

WHITE_LIST = ['get_friends_list', 'get_address', 'get_phone_number']

def request_rest_api(request):
    function_name = request.POST.get('function_name', '')

    # 👿 사용자에게 전달받은 함수명 검증하지 않고 실행
    ret = exec('{}()'.format(function_name))
    return render(request, '/success', {'data':ret})
    # 😇 함수명을 화이트리스트로 제한
    if function_name in WHITE_LIST:
        ret = exec('{}()'.format(function_name))
        return render(request, '/success', {'data':ret})
    
    return render(request, '/error', {'error':'허용되지 않은 함수입니다.'})
```
### 3. 경로 조직 및 자원 삽입
{{<boxmd>}}
검증되지 않은 외부 입력값을 통해 파일 및 서버 등 시스템 자원(파일, 소켓의 포트 등)에 대한 접근 혹은 식별을 허용할 경우, 입력 값 조작을 통해 시스템이 보호하는 자원에 임의로 접근할 수 있는 보안약점. 공격자는 이를 통해 자원의 수정-삭제, 시스템 정보 누출, 시스템 자원간 충돌로 인한 서비스 장애를 유발시킬 수 있다.
{{</boxmd>}}
Python에서 주의해야할 점
- subprocess.popen()과 같이 프로세스를 여는 함수
- os.pipe()처럼 파이프
- socket 연결 등에서 외부 입력값을 검증 없이 사용할 경우
#### 👮‍♀️ 시큐어 코딩
- 외부의 입력 값을 자원의 식별자로 사용하는 경우, 적절한 검증을 거치도록 하거나, 사전에 정의된 리스트에서 선택되도록 한다. 
- 외부의 입력이 파일명인 경우에는 경로 순회(directory traversal) 공격의 위험이 있는 문자(/, \, .. 등)를 제거할 수 있는 필터 이용한다.

1. 경로 조작 예제
```python
import os
from django.shortcuts import render

def get_info(request):
    # 외부로부터 파일명을 입력받고 있다
    request_file = request.POST.get('request_file')

    filename, file_ext = os.path.splitext(request_file)
    file_ext = file_ext.lower()

    if file_ext not in ['.txt', '.csv']:
        return render(request, '/error.html', {'error':'파일을 열 수 없습니다'})

    # 😇 파일명에서 경로 조작 문자열을 필터링
    filename = filename.replace('.', '')
    filename = filename.replace('/', '')
    filename = filename.replace('\\', '')

    with open(request_file) as f:
        data = f.read()

    return render(request, '/success.html', {'data':data})
```
2. 자원 삽입 예제
```python
import socket
from django.shortcuts import render

ALLOW_PORT = [4000, 6000, 9000]

def get_info(request):
    port = request.POST.get('port')
    
    # 😇 사용 가능한 포트 번호를 화이트리스트로 제한
    if port not in ALLOW_PORT:
        return render(request, '/error', {'error':'소켓연결 실패'})

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.bind(('127.0.0.1', port))
        ...
        return render(request, '/success')
    return render(request, '/error', {'error':'소켓연결 실패'})
```
## 보안기능
## 시간 및 상태
## 에러처리
## 코드오류
## 캡슐화
## API 오용

## 참고자료

Python 시큐어코딩 가이드(전자정부보호팀,2022.04.12)
https://www.boho.or.kr/data/guideView.do?bulletin_writing_sequence=66641 