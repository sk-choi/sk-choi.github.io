---
title : "미래내일 일경험 인턴 4일차"
date : 2024-04-20 22:01:23 +/-TTTT
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

### 데이터 수집 방식에 대해

팀원들과 회의한 결과를 보고 드렸고, 대표님께서 데이터 수집 방식에 대해

크롤링을 해보라고 하신 이후에 어떻게 크롤링을 할지 생각을 많이 해봤다.

그리고 4일차 되던 날에 대충 3년치 데이터를 크롤링 할 수 있는 코드를 완성했다.

나는 처음에 함수별로 속성 별 데이터를 끌어오는 방식을 생각했는데,

다른 분이 `json.loads()` 방식을 통해 한 번에 데이터를 끌어오는 방식을 고안하셔서 그 방향대로 가기로 했다.

근데 코드를 짜면서 한 페이지의 정보를 크롤링 하면 이전 날짜의 정보를 크롤링하게끔 코드를 작성해야 했는데

이 부분에서 헷갈려서 머리 좀 싸메다가 이중 for문 방식으로 한 페이지 정보 크롤링을 완료해서 

완료 이후에 에러 메세지가 출력되면 그 페이지의 크롤링을 멈추고 이전 날짜로 넘어가게 코드를 작성했다.

`try ~ except`문을 활용해서 구현을 할 수 있었다.

* * *

### 구현한 코드

```python
for i in range(3): #시험삼아 3일치 데이터만 추출해보았음

    for i in range(100):

        print(i)
        try:

            ccc = '//*[@id="patient_total_vital_check"]/tbody/tr[{}]'.format(i)  # 환자 정보

            value = driver.find_elements(By.XPATH, ccc)

            for element in value:
                input_val = value[0].get_attribute('data-info')

                aa = json.loads(input_val) #json으로 변환해서 저장

            # palnumb : 환자 번호, pamname : 이름, ndndate : 측정 날짜, ndnbphg : 수축기 혈압, ndnbplw : 아완기 혈압
            # ndnpuls : 맥박, ndnblsg : 혈당, ndntprt : 체온, ndnbrth : 호흡, ndnwght : 체중
                
                print(aa["palnumb"], ":", aa["pamname"], ":", aa["ndndate"], ":", aa["ndnbphg"], ":", aa["ndnbplw"],
                  ":", aa["ndnpuls"], ":", aa["ndnblsg"], ":", aa["ndntprt"], ":", aa["ndnbrth"], ":", aa["ndnwght"])
                
                #바로 db에 저장하는 방식으로 수정할 예정
                    
        except:
            break;
    driver.find_element(By.XPATH, '//*[@id="div_total_vital_check"]/div[2]/div/div[5]/div[1]/span[1]').click()
    print(" 다음 페이지 이동")
    driver.implicitly_wait(3)
```

* * *

### 앞으로의 진행 방향에 대해서 

이제는 크롤링한 데이터를 DB에 넣어볼 생각이다.

처음에 나는 데이터 프레임 형식으로 파일을 저장해서 csv파일로 만들어 학습 데이터를 만들 수 있지 않을까 생각을 해 봤는데,

팀원 분의 얘기를 들어보니 이 과정이 비효율적이라는 것을 알게 되었다.

팀원 분께 여쭤보니 크롤링한 데이터를 바로 데이터 베이스에 업로드 할 수 있다고 해서 

이 방향으로 데이터를 저장할 것 같다.

이제는 데이터를 시각화하는 방법, 인공지능에게 학습시키는 방법에 대해서 고민할 차례인 것 같다. 

&nbsp;