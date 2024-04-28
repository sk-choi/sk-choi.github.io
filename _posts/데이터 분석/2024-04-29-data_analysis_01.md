---
title : "Pandas 활용 팁 정리 01"
date : 2024-04-29 01:28:30 +/-TTTT
categories : 
- Data_Analysis
tags : 
- [데이터분석, pandas] #소문자만 가능
published : true
#permalink : categories/data_analysis

#header :
  #teaser : 

toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
---

### Intro

![img](data:image/png;base64,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)

파이썬 라이브러리에서 판다스(Pandas)는 데이터 분석을 하기 위해 사용할 수 있는

유용한 라이브러리라고 말할 수 있다.

판다스는 데이터프레임(DataFrame)이라는 형식을 통해

데이터를 가공할 수 있다.

글을 본격적으로 서술하기 전에,

판다스는

```python
import pandas as pd
```

라는 alias를 통해 `pd`로 선언이 가능하다.

오늘 이 글에서는 판다스 라이브러리의 간단한 기능에 대해서 말해 볼 것이다.

* * *

### 01\. csv파일 DataFrame으로 읽기(read_csv)

csv파일을 DataFrame으로 읽어 들이려면 다음과 같이 선언할 수 있다.

```python
df = pd.read_csv("파일경로", encoding = 'utf-8')
```

&nbsp;

read_csv의 기본 형식은 아래의 판다스 공식 페이지에서 확인할 수 있다.

https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html  

&nbsp;

`read_csv()`에서 `encoding` 파라미터의 경우, 기본값이 utf-8로 설정되어 있다.

`low_memory` 옵션의 경우 대용량의 데이터를 불러오는 경우, 칼럼별 데이터 타입을 추측하는데 많은 

메모리 소비가 발생할 수 있기에, `False`로 설정하는 것이 좋다.

특히 특정 컬럼에 `NaN` 값이 포함되어 있거나 여러 형태의 자료형이 섞여 있으면 `DtypeWarning`이 발생하므로

이런 경우에도 `low_memory`옵션을 `False`로 설정하는 것이 좋다.

* * *

### 02\. 서로 다른 데이터 프레임 결합하기(concat)

서로 다른 데이터 프레임을 결합하기 위해서는 `concat()`을 사용해야 한다.

```python
pd.concat([df1, df2], axis = 1)
```

`concat`의 사용 예시는 다음과 같다.

`concat` 사용 시 주의해야 할 점은 결합할 데이터 프레임이 서로 같은 형태여야 한다는 것이다.

`axis` 옵션의 경우, 0 인 경우에는 데이터 프레임을 위 아래로 합치고,

1인 경우에는 데이터 프레임을 왼쪽 오른쪽으로 합친다.

찾아보니 `join` 옵션도 존재하는데 `outer, inner`로 선언이 가능하고

`outer`는 합집합, `inner`는 교집합을 의미한다.

* * *

### 03\. 데이터프레임 병합하기(merge)

`merge`는 `concat`과는 다르게 각 데이터 프레임에 존재하는 고유의 키 값을 중심으로

데이터 프레임을 병합하는 기능을 수행한다.(어떻게 보면 SQL의 join과 비슷한 기능인 것 같다.)

`merge`의 사용예시는 다음과 같다.

```python
result = pd.merge(df1, df2, on='id', how='left')
```

`merge`의 기본적인 사용 옵션에는 `on`과 `how`가 존재하는데,

`on`은 어떤 키를 중심으로 데이터 프레임을 합칠지를 의미하고,

`how`는 어떤 방식으로 병합(join)을 수행할지를 의미한다.

`how`의 옵션에는 `left, right, outer, inner, cross`가 존재하며 디폴트 값은 `inner`이다.

이 옵션은 SQL의 조인 방식을 선택하는 것과 비슷하다.

* * *

### 04\. NaN값이 들어있는 특정 행, 열 지우기(dropna)

데이터프레임의 특정 행이나 열에 `NaN` 타입의 데이터가 있을 경우, 코드 실행 중에 오류가 발생할 수 있다.

이를 위해서 `dropna`를 통해 `NaN`값이 들어있는 행이나 열을 지울 수 있다.

사용예시는 다음과 같다.

```python
df = df.dropna(axis = 0)
```

여기서 `axis`를 0으로 선언하면 특정 행을 지우겠다는 의미이고,

1로 선언을 할 경우 특정 열을 지우는 것을 의미한다.

* * *

### 05\. 데이터 프레임 자료형 변환(astype)

데이터 프레임을 만들었을 때, `df.info()`를 사용하면 각 컬럼 별 데이터 형식을 알 수 있다.

이 때 데이터 분석의 목적에 맞게 데이터의 형식을 변환해야 할 경우가 생길 수 있는데,

이것을 위해 `astype`을 사용할 수 있다.

`astype`의 사용 예시는 다음과 같다.

```python
df['A'] = df['A'].astype('int64')
```

이것은 A라는 열의 데이터 형식을 int64 타입으로 변환하는 것을 의미한다.

최근에 데이터 가공을 하다가 알게 된 사실인데,

데이터프레임은 Datetime이라는 날짜를 저장하는 자료형도 존재한다.

그런데 만약 날짜가 저장된 열을 datetime이라는 형식으로 변환해야 할 경우,

```python
df['날짜'] = df['날짜'].astype('int').astype('str')
df['날짜'] = pd.to_datetime(df['날짜']) #날짜 datetime으로 변환
```

이런 방식으로 변환을 해야 변환을 잘 이루어졌다.

```python
df['날짜'] = pd.to_datetime(df['날짜'], format='%Y%m%d',errors='raise')
```

참고로 내가 사용한 다른 방법은 이것인데, 이렇게 `format`으로 년도, 월, 일을 지정할 수 있다.