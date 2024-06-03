---
title : "matplotlib 그래프에서 한글 깨질 때 해결 방법(font_manager, rc)"
date : 2024-06-04 01:35:30 +/-TTTT
categories : 
- Data_Analysis
tags : 
- [데이터분석, matplotlib] #소문자만 가능
published : true
#permalink : categories/data_analysis

#header :
  #teaser : 

toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
---

### matplotlib의 한글 깨짐 문제 발생

matplotlib으로 그래프를 출력할 때 한글이 들어가게 될 경우

한글이 깨져서 출력되는 상황이 발생한다. 

matpltlib에서는 기본적으로 한글폰트가 제공되지 않기 때문이다.

이를 해결하기 위해서는 아래와 같이 `font_manager`와 `rc`를 import하면 된다.

* * *

### 예시코드

```python
import matplotlib.pyplot as plt
from matplotlib import font_manager, rc

font_name = font_manager.FontProperties(fname="c:\Windows\Fonts\malgun.ttf").get_name()
rc('font', family=font_name)
```

* * *

### 참고자료 읽어보기

https://matplotlib.org/stable/api/font_manager_api.html

해당 matplotlib 공식 문서에 들어가서 확인 해보니, font_manager의 findfont 함수가 FontProperties와 일치하는 시스템 글꼴 경로에 있는

TTF타입의 글꼴을 확인하고, 반환하는 역할을 한다.

그리고 `get_name()`은 폰트 속성과 가장 잘 맞는 폰트의 이름을 반환한다.

그리고 `rc()`를 통해서 폰트를 설정하는 것이다.

정리하자면, `font_manager`의 `FontProperties`를 통해서 로컬 혹은 시스템에 들어있는 폰트 관련 경로를 불러오고,

그것을 `rc`를 통해서 `matplotlib`에 사용할 폰트로 지정하는 것이라 할 수 있다.

&nbsp;