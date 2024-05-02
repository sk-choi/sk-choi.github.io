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

이 글에서는 Pandas 라이브러리의 `corr` 메서드, `drop` 메서드, `columns` 메서드에 대해서 다룹니다.

* * *

### 01\. 데이터프레임의 컬럼별 상관관계 분석하기(corr)

데이터 프레임의 컬럼별 상관관계를 분석하고 싶다면 

```python
df.corr()

print(df.corr())
```

위와 같은 선언을 통해서 `corr()` 메서드를 통해 컬럼별 상관관계를 확인할 수 있다.

상관관계는 -1 ~ 1 사이의 값을 가지는데, 양수일 경우 양의 상관관계, 음수일 경우 음의 상관관계, 0은 상관관계를 갖지 않는다고 볼 수 있다.

`corr()` 메서드의 기본적인 옵션에는 method가 있는데 `pearson, kendall, spearman`이라는 방식을 각각 선택할 수 있다.

* * *

### 02\. 특정 칼럼 삭제하기(drop)

drop은 기본적으로 데이터 프레임에서 특정 칼럼을 없앨 때 사용한다.

drop에는 여러 옵션이 존재하는데, `labels, axis, index, columns, inplace, errors` 등의 옵션이 존재한다.

`labels`: 삭제할 레이블, axis 지정 필요

`axis`: 0일 경우 index, 1일 경우 columns를 의미한다.

`index`: index명을 통해 바로 삭제 가능

`columns`: columns명을 통해 바로 삭제 가능

`inplace`: 원본을 변경할지 정하는 옵션. True일 경우 원본이 변경

`errors`: 삭제할 행이나 열이 없을 때, 오류를 띄울지 결정하는 옵션.

예시는 아래와 같다.

```python
data.drop(data[data['A'] == 0].index, inplace=True)
```

* * *

### 03\. 데이터 프레임의 컬럼 확인하기(columns)

데이터 프레임의 컬럼을 확인하기 위해선 다음과 같은 선언을 통해 확인이 가능하다.

```python
df.columns
```

예시는 아래와 같다.

```python
import pandas as pd

df = {'A': [1,2,3],
      'B': [4,5,6],
      'C': [7,8,9] }

df = pd.DataFrame(df)

print(df.columns)

# Index(['A', 'B', 'C'], dtype='object') 출력
```
