---
title : "데이터베이스 SQL 구문 기초08(인라인 뷰와 스칼라, DECODE, PIVOT)"
date : 2024-11-21 21:34:30 +/-TTTT
categories : 
- Database
tags : 
- [데이터베이스] #소문자만 가능
published : true
#permalink : categories/algorithm

#header :
  #teaser : 

toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
---


### 서브 쿼리 인라인 뷰

인라인 뷰는 FROM 절에서 사용 가능한 일회성의 뷰 성격을 가진 서브쿼리를 의미한다.

```sql
SELECT EMPNO, ENAME, (SAL * 12 ) + 100 AS YSAL

FROM EMP

WHERE (SAL + 12) + 100 > 3000;
```

이 쿼리를 아래와 같이 변경할 수 있다.

```sql
SELECT * FROM (

SELECT EMPNO, ENAME, (SAL + 12) + 100 AS YSAL

FROM EMP)

WHERE YSAL > 3000;
```

&nbsp;

* * *

### 스칼라 : SELECT절 서브 쿼리

스칼라는 SELECT절의 서브 쿼리를 말하며

이는 통계성, 대시보드용으로 적합하다.

부서별로 COUNT해서 출력

```sql
SELECT (SELECT COUNT(EMPNO) FROM EMP WHERE DEPTNO = 10) AS CNT10,

(SELECT COUNT(EMPNO) FROM EMP WHERE DEPTNO = 20) AS CNT20

FROM DUAL;
```

* * *

### PIVOT과 DECODE

**PIVOT :** 가로와 세로를 바꾸는 것.

`PIVOT ~ FOR`형식으로 절처럼 사용한다.

&nbsp;

**DECODE :** PIVOT처럼 가로와 세로를 바꾼다. 함수로 사용한다.

`DECODE(A,B,X,Y)`

> A = B 이면 X출력
> 
> A != B 이면 Y출력

&nbsp;

`DECODE(A,B,X,C,Y,Z)`

> A=B -> X출력
> 
> A=C -> Y출력
> 
> A!=B 그리고 A!=C - > Z출력

&nbsp;

**DECODE 사용 방법**

```sql
SELECT SUM(DECODE(DEPTNO, 10, 1, 0)) AS CNT10,

SUM(DECODE(DEPTNO, 20, 1, 0)) AS CNT20,

SUM(DECODE(DEPTNO, 30, 1, 0)) AS CNT30

FROM EMP;
```

&nbsp;

**PIVOT 사용 방법**

```sql
SELECT * 

FROM (

SELECT JOB, TO_CHAR(HIREDATE, 'MM') AS HM FROM EMP

)

PIVOT (

COUNT(*)

FOR HM IN ('10', '20', '30')

);
```

* * *

### QUIZ

**1980~1982 사이에 입사한 각 부서별 사원수를 부서번호, 부서명, 입사년도를 출력해라**

```sql
SELECT D.DEPTNO, D.DNAME,

SUM(DECODE(TO_CHAR(E.HIREDATE, 'YYYY'), '1980', 1, 0)) AS 입사1980,

SUM(DECODE(TO_CHAR(E.HIREDATE, 'YYYY'), '1981', 1, 0)) AS 입사1981,

SUM(DECODE(TO_CHAR(E.HIREDATE, 'YYYY'), '1982', 1, 0)) AS 입사1982

FROM EMP E, DEPT D

WHERE E.DEPTNO = D.DEPTNO

GROUP BY D.DEPTNO, D.DNAME;
```

처음에 어떻게 해야 할 지 몰라서 스칼라로 풀려고 했지만,

알고 보니 DECODE를 이용해서 푸는 거였다.

DECODE는 스위치 같은 것이다.(값 비교)