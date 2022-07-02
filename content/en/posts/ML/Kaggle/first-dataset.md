---
title: "웹툰 데이터 수집하기"
date: 2022-06-15T22:19:06+09:00
draft: true
tags: ["ml", "kaggle"]
description: 데이터셋이 없다면 내가 직접 수집해서 만들기
---

## Kaggle Dataset
[Webtoon Dataset in Korean](https://www.kaggle.com/datasets/bmofinnjake/naverwebtoon-datakorean)
## 네이버 연재 만화
### step0. 기본 설정, 필요 모듈
```python
import re
import csv
import requests
from bs4 import BeautifulSoup

naver_url = 'https://comic.naver.com'
index = ['가','나','다','라','마','바','사','아','자','차','카','타','파','하','A','1']
```
### step1. 웹툰 목록 수집
[작품별 목록 페이지](https://comic.naver.com/webtoon/creationList?prefix=%ea%b0%80&view=list)에서 각 인덱스(ㄱ,ㄴ,ㄷ..)별 웹툰 정보 `extract_outline()`으로 추출
```python
def get_outline():
    toons = [] # []
    for order in index:
        url = naver_url + '/webtoon/creationList?prefix={0}&view=list'.format(order)
        html = requests.get(url)
        soup = BeautifulSoup(html.text, 'html.parser')

        for item in soup.find_all('tr'):
            toon = extract_outline(item)
            if not toon: continue
            toons.append(toon)

    # 완결웹툰 리스트 중 [드라마원작] 웹툰
    originals = extract_originals()
    for original in originals:
        toons.append(original)
    return toons
```
#### 웹툰 기본 정보
목록에서 `링크`, `제목`, `별점`, `작가` 정보 추출
```python
def extract_outline(item):
    a = item.find('a', href=True)
    if not a: return

    titleId = int(re.findall("\d+", a['href'])[0])
    title = item.find('strong').string
    end = True if "(완결)" in item.find('td', class_="subject").get_text() else False
    rating = float(item.find('div', class_="rating_type").find('strong').string)
    date = item.find('td', class_="date").string.strip()

    return {
        'id': titleId,
        'title': title,
        'rating': rating,
        'completed': end,
        'date': date,
        'link': naver_url + a['href']
    }
```
#### 누락된 드라마 원작 웹툰 정보
완결 웹툰 리스트에서 [드라마원작] 말머리를 같는 웹툰 정보 추출
```python
def extract_originals():
    url = 'https://comic.naver.com/webtoon/finish?view=list'
    html = requests.get(url)
    soup = BeautifulSoup(html.text, 'html.parser')
    originals = []

    for item in soup.find_all('tr'):
        a = item.find('a', href=True)
        if not a: continue
        title = item.find('strong').string
        if '[드라마원작]' not in title: continue
        titleId = int(re.findall("\d+", a['href'])[0])
        rating = float(item.find('div', class_="rating_type").find('strong').string)
        date = item.find('td', class_="date").get_text().split()[0] # diff

        result= {
            'id': titleId,
            'title': title,
            'rating': rating,
            'completed': True,
            'date': date,
            'link': naver_url + a['href']
        }
        originals.append(result)
    
    return originals
```
### step2. 상세 정보
웹툰 상세페이지에 있는 정보(작가, 장르, 
```python
def extract_detail(id, url):
    html = requests.get(url)
    html.close()
    soup = BeautifulSoup(html.text, 'html.parser')
    thumb = soup.find('div', class_="thumb")
    detail = soup.find('div', class_="detail")
    # likes = detail.find('em', class_="u_cnt").string
    age = detail.find('span', class_="age")
    banner = soup.find("tr", class_="band_banner")
    free = True if banner and 'https://play.google.com/' in banner.find("a")["href"] else False
    
    return {
        'id': id,
        'title': detail.find('span', class_="title").string,
        'author': detail.find('span', class_="wrt_nm").string.strip(),
        'genre': detail.find('span', class_="genre").string,
        'description': detail.find('p').get_text(),
        'age': age.string if age else None,
        'free': free,
        'thumbnail': thumb.find('img')['src']
    }

def get_detail(simple_file):
    f = open(simple_file, 'r')
    toons = csv.DictReader(f)
    toon_info = []
    for toon in toons:
        detail = extract_detail(toon['id'], toon['link'])
        toon_info.append(detail)
    f.close()
    return toon_info
```
### step3. 파일 저장
```python
import datetime
import csv

def save_csv(data, file_name):
    # time = datetime.datetime.now().strftime("%Y%m%d-%H%M%S")
    file = open("datasets/"+file_name+".csv", mode="w", encoding="UTF8")
    writer = csv.writer(file)
    writer.writerow(list(data[0].keys()))
    for toon in data:
        writer.writerow(list(toon.values()))
    file.close()
    print(f"...{len(data)}라인이 /datasets/{file_name}.csv에 저장되었습니다.")
    return 0
```
## 베스트도전 만화

### step0. 기본 설정, 필요 모듈
```python
import re
import csv
import requests
from bs4 import BeautifulSoup

challenge_url = 'https://comic.naver.com/genre/bestChallenge'
fomats = ['episode', 'omnibus', 'story']
genres = ['daily', 'comic', 'fantasy', 'action', 'drama', 'pure', 
         'sensibility', 'thrill', 'historical', 'sports']
```

### step1. 끝 페이지 알아내기
장르별로 마지막 페이지 인덱스 범위를 알아야 해당 장르의 모든 웹툰들을 수집할 수 있다
```python
def extract_pages(genre, page_num):
    url = challenge_url + '?m={0}&page={1}'.format(genre, page_num)
    resut = requests.get(url)
    soup = BeautifulSoup(resut.text, "html.parser")
    pagination = soup.find("div", {"class": "paginate"})

    links = pagination.find_all('em')
    pages = []
    for link in links:
        pages.append(int(link.string))
    max_page = pages[-1]
    return max_page

def get_lastpage_num(genre):
    curr_num, end_num = 1, -1
    while True:
        end_num = extract_pages(genre, curr_num)
        if curr_num == end_num:
            break
        curr_num = end_num
    return end_num
```
### step2. 웹툰 기본 정보 수집
웹툰id, 제목, 작가, 요약, 평점, 장르 
```python
def get_outline(gnr, page_num):
    global toons
    url = challenge_url + '?m={0}&page={1}'.format(gnr, page_num)
    html = requests.get(url)
    soup = BeautifulSoup(html.text, 'html.parser')

    items = soup.find_all('div', {"class":"challengeInfo"})
    
    for item in items:
        a = item.find('a', href=True)
        titleId = int(re.findall("\d+", a["href"])[0])
        title = a.string
        author = item.find('a', {"class":"user"}).text
        summary = item.find('div', {"class":"summary"}).text
        rating = float(item.find('div', {"class":"rating_type"}).strong.text)

        toons.append({
            "id": titleId,
            "title": title,
            "author": author,
            "summary": summary,
            "rating": rating,
            "genre": gnr
        })
```
### step3. 상세 정보 추가
웹툰 구성방식, 설명, 정식연재된 도전웹툰 여부, 포텐업 선정 여부, 링크 추가
```python
def get_detail(record):
    # 웹툰 페이지로 가면 전개방식을 볼 수 있다
    # 포텐업, 정식연재 여부
    url = 'https://comic.naver.com/bestChallenge/list?titleId={0}'.format(record["id"])
    html = requests.get(url, headers={'User-Agent':'Mozilla/5.0'})
    soup = BeautifulSoup(html.text, 'html.parser')

    gnr = soup.find('div', {"class":"snb"}).find("li", {"class":"on"}).a.text # 전개방식
    item = soup.find('div', {"class":"comicinfo"})
    thumb = item.find('div', {"class":"thumb"})

    serialize = True if thumb.find('span', {"class":"mark_serial"}) else False # 정식연재 여부
    potenup = True if thumb.find('span', {"class":"mark_poten"}) else False # 포텐업 여부

    detail = item.find('div', {"class":"detail"})
    description = detail.find('p').text # 웹툰 설명

    # 웹툰 정보 업데이트
    # record.update({"format": gnr, "description": description, "serialize": serialize, "potenup": potenup})
    return {
        "id": record["id"],
        "title": record["title"],
        "author": record["author"],
        "summary": record["summary"],
        "rating": record["rating"],
        "genre": record["genre"],
        "format": gnr,
        "description": description,
        "serialize": serialize,
        "potenup": potenup,
        "link": url
    }
```
### step4. 파일로 저장
```python
import csv

def save_csv(data, file_name):
    # time = datetime.datetime.now().strftime("%Y%m%d-%H%M%S")
    file = open("./"+file_name+".csv", mode="w", encoding="UTF8")
    writer = csv.writer(file)
    writer.writerow(list(data[0].keys()))
    for toon in data:
        writer.writerow(list(toon.values()))
    file.close()
    print(f"...{len(data)}라인이 /datasets/{file_name}.csv에 저장되었습니다.")
    return 0
```