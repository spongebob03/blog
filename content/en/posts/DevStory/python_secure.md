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
### 4. 크로스사이트 스크립트(XSS)
Cross-site scripting Attacks
{{<boxmd>}}
웹사이트의 결함을 이용하여 악성코드(일반적으로 클라이언트 측 js사용)삽입하여 사용자에게 보내는 공격 방법. 일반적으로 애플리케이션 사용자를 목표로 삼는다.
{{</boxmd>}}
공격자가 웹 응용프로그램을 속여 사용자의 브라우저에서 실행할 수 있는 형식의 데이터를 보낼때 발생. 일반적인 HTML+공격자의 XSS코드 조합 뿐만 아니라 악성코드 다운로드, 플로그인 또는 미디어 콘텐츠를 이용하기도 한다. 입력 양식의 데이터 또는 서버에서 클라이언트 endpoint로 전달된 데이터가 적절한 검증 없이 사용자에게 표시되도록 허용하는 경우 발생.
#### 유형
- Reflective XSS (Non-persistent XSS)
공격 코드를 사용자 HTTP 요청에 삽입 -> 해당 공격 코드가 서버 응답 내용에 그대로 반사(Reflected)되어 브라우저에서 실행시키는 공격기법. 종종 악의적으로 제작된 링크를 클릭하도록 사용자를 속여 대상 사이트로 데이터를 보내도록 한다. 대부분 공개적으로 게시되거나 피싱(phishing) 이메일 또는 단축 URL, 모호한 URL에 매개 변수로 포함하는 방법이 쓰인다.
- Rersistent XSS (Stored XSS)
신뢰할 수 없거나 확인되지 않은 사용자 입력이 대상 서버에 저장될 때 발생. 게시판 글, 댓글 또는 방문자 로그가 일반적인 공격 대상이 된다. 인증되거나 인증되지 않은 다른 사용자가 공격자의 악성 콘텐츠를 보게된다. SNS 및 회원 그룹에서 흔히 볼 수 있는거 같이 공개적으로 표시되는 프로필 페이지가 쉬운 공격 대상 중 하나이다.
- DOM 기반 XSS (Client-Side XSS)
공격 스크립트가 포함된 악성 URL을 통해 전달되는 경우가 많다. 문선 개체 모델을 수정하여 브라우저에서 독립적으로 완전한 공격을 실행해서 웹 페이지에 있는 js의 검증 로직을 무효화한다.공격자는 세션 정보를 포함한 쿠키와 같은 개인 정보를 피해자의 컴퓨터에서 공격자에게 전송할 수 있다. 공격자는 피해자의 정보를 사용하여 웹 사이트에 악의적인 요청을 보낼 수 있다.  
#### 👮‍♀️ 시큐어 코딩
- 외부 입력값 또는 출력값에 스크립트가 삽입되지 못하도록 문자열 치환 함수를 사용하여 &<>*‘/()등을 &amp; &lt; &gt; &quot; &#x27; &#x2F; &#x28; &#x29;로 치환하거나, html라이브러리의 escape()를 사용한다. 
- HTML 태그를 허용하는 게시판에서는 허용되는 HTML 태그들을 화이트리스트로 만들어 해당 태그만 지원하도록 한다.
```python Django 예제
from django.shortcusts import render
from django.utils.safestring import mark_safe

def profile_link(request):
    # 외부 입력값을 검증 없이 HTML 태그 생성의 인자로 사용
    profile_url = request.POST.get('profile_url')
    profile_name = request.POST.get('profile_name')

    object_link = '<a href="{}">{}</a>'.format(profile_url, profile_name)

    # 😱 mark_safe함수는 Django의 XSS escape 정책을 따르지 않는다
    object_link = mark_safe(object_link)

    # 😇 mark_safe 함수 사용하지 않는다

    return render(request, 'my_profile.html',{'object_link':object_link})
```
템플릿 코드
```html
<!doctype html> <html>
    <body>
        <div class="content">
            // 😱 autoescape off로 해당 블록내의 데이터는 XSS 공격에 노출될 수 있음
            {% autoescape off %}
            {{ content }}
            {% endautoescape %}

            // 😇 autoescape on 로 해당 블록내의 데이터는 XSS 공격에 노출되지 않음
            {% autoescape off %}
            {{ content }}
            {% endautoescape %}
        </div> 
        <div class="content2">
            // 😱 safe 필터 사용으로 XSS 공격에 노출될 수 있다
            {{ content | safe }}

            // 😇 검증되지 않은 데이터에는 safe 필터 ❌
            [{ content }]
        </div>
    </body>
</html>
```
```python
import html
from flask import Flask, request, render_template

@app.route('/search', methods=['POST'])
def search():
    seasrch_keyword = request.from.get('search_keyword')

    # 😱 사용자의 입력을 아무런 검증 없이 치환 없이 동적 웹 페이지에 사용하고 있다
    return render_template('search.html', search_keyword=search_keyword)

    # 😇 동적 웹 페이지 생성ㅇ에 사용되는 데이터는 HTML 엔티티코드로 치환
    escape_keyword = html.escape(search_keyword)
    return render_template('search.html', search_keyword=search_keyword)
```
### 5. 운영체제 명령어 삽입
{{<boxmd>}}
검증을 거치지 않은 사용자 입력값이 운영체제 명령어의 일부 또는 전부로 구성되어 실행 -> 의도하지 않은 시스템 명령어가 실행 -> 부적절하게 권한이 변경되거나 시스템 동작, 운영에 악영향
{{</boxmd>}}
명령어 라인의 파라미터나 스트림 입력 등 외부 입력을 사용하여 시스템 명령어를 생성하는 프로그램 많이 있다. 하지만 이런 경우 외부 입력 문자열은 신뢰할 수 없기 때문에 적절한 처리를 해주지 않으면 보안에 취약하다.
Python에서 주의할 점  
String으로 표현된 식을 인수로 받아 반환하는 eval() 함수 + 인수로 받은 문자열을 실행하는 exec() 함수를 같이 사용하면 여러 변수들에 동적으로 값을 할당하여 사용될 수 있어 취약하다.
#### 👮‍♀️ 시큐어 코딩
- 외부 입력값이 시스템 명령에 포함되는 경우 |, ;, &, :, >, <, `(backtick), \, ! 과 같이 멀티라인 파이프 리다이렉트 문자 등을 필터링 하고 명령을 수행할 파일명, 옵션을 제한하여 인자로만 사용될 수 있도록 한다
- 외부 입력에 따라 명령어를 생성하 거나 선택이 필요한 경우에는 명령어 생성에 필요한 값 들을 미리 지정해 놓고 외부 입력에 따라 선택하여 사용한다.

1. os.system
```python
import os
from django.shortcuts import render

# 화이트리스트의 파라미터 배열 정의
ALLOW_PROGRAM = ['notepad', 'calc']

def execute_command(request):
    app_name_string = request.POST.get('app_name')

    # 입력받은 파라미터 사용가능한 시스템 명령어 일부로 제한하여 사용
    if app_name_string not in ALLOW_PROGRAM:
        return render(request, '/error.html', {"ERROR":"허용되지 않은 프로그램"})

    os.system(app_name_string)
    return render(request, '/success.html')
```
2. subprocess() 함수
```python
import subprocess
from django.shortcuts import render

def execute_command(request):
    date = request.POST.get('date', '')

    # 😱 입력 파라미터를 제한하지 않아 전달된 모든 프로그램 실행될 수 있음
    cmd_str = "cmd /c backuplog.bat " + data
    subprocess.run(cmd_str, shell=True)
    return render(request, '/success.html')

    # 😇 명령어를 추가로 실행 or 다른 명령이 실행될 수 있는 키워드 예외처리
    for word in ['|', ';', '&', ':', '>', '<', '`', '\\', '!']:
        date = date.replace(word, '')
    subprocess.run(["cmd", "/c", "backuplog.bat", date])
    return render(request, '/success.html')
```
### 6. 위험한 형식 파일 업로드
{{<boxmd>}}
서버에서 실행될 수 있는 스크립트 파일(.asp, .jsp, .php, .sh 등)을 업로드 할 수 있고 공격자가 웹을 통해 직접 실행시킬 수 있는 경우 시스템 내부 명령어를 실행하거나 외부와 연결하여 시스템을 제어할 수 있는 보안 약점
{{</boxmd>}}
#### 👮‍♀️ 시큐어 코딩
- 특정 파일 유형만 허용하도록 화이트리스트 방식으로 파일 유형 제한
  - Content-Type도 확인
  - 파일 크기 및 파일 개수 제한
- 업로드된 파일을 웹 루트 폴더 외부에 저장하여 공격자가 URL을 통해 파일을 실행할 수 없도록 한다
- 업로드 되는 파일 이름은 공격자가 추측할 수 없는 무작위한 이름으로 변경하여 저장
- 업로드된 파일 저장할 경우 최소 권한만 부여
```python
import os
from django.shortcuts import render
from django.core.files.storage import FileSystemStorage

# 업로드 하는 파일에 대한 개수, 크기, 확장자 제한
FILE_COUNT_LIMIT = 5
# 업로드 파일의 최대 사이즈 제한
FILE_SIZE_LIMIT = 5 * 1024 * 1024
# 화이트리스로 허용하는 확장자 관리
WHITE_LIST_EXT = ['.jpg', '.jpeg']

def file_upload(request):
    if request.FILES['upload_file']:
        upload_file = request.FILES['upload_file']
        fs = FileSystemStorage(location='media/screenshot', base_url='media/screenshot')

        filename = fs.save(upload_file.name, upload_file)
        return render(request, '/success.html', {'filename':filename})
    return render(request, '/error.html', {'error':'파일 업로드 실패'})

def file_upload(request):
    if len(request.FILES) == 0 or len(request.FILES) > FILE_COUNT_LIMIT:
        return render(request, '/error.html')

    for filename, upload_file in request.FILES.items():
        if upload_file.content_type != 'image/jpeg':
            return render(request, '/error.html')
        if upload_file.size > FILE_SIZE_LIMIT:
            return render(request, '/error.html')
        file_name, file_ext = os.path.splitext(upload_file.name)
        if file_ext.lower() not in WHITE_LIST_EXT:
            return render(request, '/error.html')

    fs = FileSystemStorage(location='media/screenshot', base_url = 'media/screenshot')
    for upload_file in request.FILES.values():
        fs.save(upload_file.name, upload_file)

    return render(request, '/success.html', {'filename': filename})
```
### 7. 신뢰되지 않는 URL주소로 자동접속 연결
{{<boxmd>}}
{{</boxmd>}}
#### 👮‍♀️ 시큐어 코딩
### 8. 부적절한 XML 외부 개체 참조
{{<boxmd>}}
{{</boxmd>}}
#### 👮‍♀️ 시큐어 코딩
### 9. XML 삽입
{{<boxmd>}}
{{</boxmd>}}
#### 👮‍♀️ 시큐어 코딩
### 10. LDAP 삽입
{{<boxmd>}}
{{</boxmd>}}
#### 👮‍♀️ 시큐어 코딩
### 11. 크로스사이트 요청 위조 CSRF
{{<boxmd>}}
{{</boxmd>}}
#### 👮‍♀️ 시큐어 코딩
### 12. 서버사이드 요청 위조
{{<boxmd>}}
{{</boxmd>}}
#### 👮‍♀️ 시큐어 코딩
### 13. HTTP 응답분할
{{<boxmd>}}
{{</boxmd>}}
#### 👮‍♀️ 시큐어 코딩
### 14. 부적절한 입력값
### 15. 포맷 스트링 삽입

## 보안기능
## 시간 및 상태
## 에러처리
## 코드오류
## 캡슐화
## API 오용

## 참고자료

Python 시큐어코딩 가이드(전자정부보호팀,2022.04.12)
https://www.boho.or.kr/data/guideView.do?bulletin_writing_sequence=66641 