---
title : "미래내일 일경험 인턴 기록18"
date : 2024-06-21 09:39:30 +/-TTTT
categories : 
- Intern
tags : 
- [인턴] #소문자만 가능
published : true
#permalink : categories/internship

#header :
  #teaser : 

toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
---

### Intro

이 글은 미래내일 일경험 인턴십에서 근무하면서

겪거나 배우고 느낀 것을 기록하고자 작성한 포스트입니다.

* * *

### 측정 안된 날부터 크롤링하는 기능 완성

예전에 추가하려고 했던 측정 안된 날부터 크롤링하는 기능을 완성했다.

```python
def cal_date():
    db = cx_Oracle.connect(user=config.서버_아이디, password=config.서버_비밀번호, dsn=config.서버_dsn)

    cursor = db.cursor()
    sql = ''' select MAX(측정날짜) from health '''
    cursor.execute(sql)
    result = cursor.fetchone()  # fetchone() 사용하여 첫 번째 결과만 가져옴
    max_date = result[0] if result else None  # 쿼리 결과가 있으면 날짜 저장
    max_date_dt = pd.to_datetime(max_date)

    # 현재 날짜 설정
    current_date = datetime.now()

    # 최대 날짜가 존재하면 비교 후 결정
    if max_date_dt == current_date:
        input_date = current_date - timedelta(days=1)  # 하루 전 날짜
    else:
        diff = current_date - max_date_dt
        #print(diff.days)
        input_date = current_date - timedelta(days=diff.days)

    # 날짜 차이 계산
    difference = current_date - input_date
    days_difference = difference.days

    print(days_difference)  # 차이 출력

    cursor.close()
    db.close()

    return days_difference
```

DB에 있는 가장 최근의 날짜가 오늘 날짜랑 같으면 오늘 날짜만 크롤링을 수행하고

아닐경우에는 최근 측정날짜 이후 날부터 크롤링을 수행한다.

* * *

### 스트림릿 프로펫 설치 및 에러 해결

[스트림릿 프로펫 설치 방법](https://github.com/artefactory/streamlit_prophet)

스트림릿 프로펫이라는 것을 설칳해서 테스트도 해 봤다. 스트림릿과 프로펫을 결합시킨 툴(?) 같은 건데

웹으로 직접 데이터를 프로펫으로 분석할 수 있다.

그런데 중간에 에러가 발생했다. 발생한 에러는 아래와 같다.

`downgrade the protobuf package to 3.20.x or lower.`

에러 해결을 위해서 참고한 링크는 아래와 같다.

[스트림릿 프로펫 에러 해결 참고](https://chaeso-coding.tistory.com/65)

위의 링크를 참고해서 `<span style="color: #abb2bf;">pip install protobuf==</span><span style="color: #d19a66;">3.19</span><span style="color: #d19a66;">.0</span>`를 실행했더니 잘 실행되었다.

&nbsp;