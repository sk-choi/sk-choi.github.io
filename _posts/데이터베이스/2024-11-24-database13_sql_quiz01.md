---
title : "SQL 퀴즈 정리 01"
date : 2024-11-24 11:14:30 +/-TTTT
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

### QUIZ01
**각 사원 별 커미션(COMM)이 0 또는 NULL이고 부서위치가 ‘GO’로 끝나는 사원의 정보를 사원번호, 사원이름, 커미션, 부서번호, 부서명, 부서위치를 출력하여라.
조건1. 커미션(COMM)이 NULL이면 0으로 출력**
```sql
SELECT FIRST.*
FROM(
SELECT E.EMPNO, E.ENAME, NVL(E.COMM,0) AS COMM, D.DEPTNO, D.DNAME, D.LOC
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO
)FIRST
WHERE FIRST.COMM = 0 AND FIRST.LOC LIKE '%GO';
```

* * *

### QUIZ02
**각 부서 별 평균 급여가 2000 이상이면 초과, 그렇지 않으면 미만을 출력하시오.**
```sql
SELECT NEW_TB.DEPTNO, NEW_TB. AVG_NEW,
CASE
WHEN AVG_NEW >= 2000 THEN '초과'
ELSE '미만'
END AS 평균급여
FROM
(
SELECT D.DEPTNO, ROUND(AVG(E.SAL),2) AS AVG_NEW
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO
GROUP BY D.DEPTNO
) NEW_TB
ORDER BY NEW_TB.DEPTNO ASC;
```

* * *

### QUIZ03
**각부서별 입사일이 가장 오래된 사원을 한 명씩 선별해 사원번호, 사원명, 부서번호, 입사일을 출력하시오.**
```sql
SELECT EMPNO, ENAME, DEPTNO, HIREDATE
FROM EMP
WHERE HIREDATE IN (
SELECT MIN(HIREDATE)
FROM EMP
GROUP BY DEPTNO
)
ORDER BY DEPTNO ASC
;
```

* * *

### QUIZ04
**EMP 테이블에서 부서 인원이 4명보다 많은 부서의 부서번호, 인원수, 급여의 합을 출력하시오.**
```sql
SELECT T1.DEPTNO, T1.COUNT, T1.SUM
FROM
(
SELECT DEPTNO, COUNT(ENAME) AS COUNT, SUM(SAL) AS SUM
FROM EMP
GROUP BY DEPTNO) T1
WHERE COUNT > 4; 
```

* * *

### QUIZ05
**EMP 테이블에서 가장 많은 사원이 속해있는 부서번호와 사원수를 출력하시오.**
```sql
SELECT DEPTNO, "COUNT(*)"
FROM
(
SELECT DEPTNO, COUNT(EMPNO) AS "COUNT(*)"
FROM EMP
GROUP BY DEPTNO
ORDER BY DEPTNO DESC)
WHERE ROWNUM = 1;

--다른 풀이 방법
SELECT DEPTNO, COUNT(1) AS CNT
FROM EMP
GROUP BY DEPTNO
HAVING COUNT(1) = (SELECT MAX(COUNT(1)) FROM EMP GROUP BY DEPTNO);
```

* * *

### QUIZ06
**EMP 테이블에서 가장 많은 사원을 갖는 MGR의 사원번호를 출력하시오.**
```sql
SELECT MGR AS EMPNO, T1.CNT 
FROM
(
SELECT MGR, COUNT(MGR) AS CNT
FROM EMP
GROUP BY MGR
ORDER BY CNT DESC) T1
WHERE ROWNUM = 1
;
```

* * *

### QUIZ07 
**가장 높은 급여를 받는 사원보다 입사일이 늦은 사원들의 이름, 입사일을 출력**
```sql
SELECT ENAME, HIREDATE
FROM EMP
WHERE HIREDATE > (SELECT HIREDATE
                    FROM EMP
                        WHERE SAL IN (SELECT MAX(SAL)
                                                FROM EMP)
)
;

--또는

SELECT ENAME, TO_CHAR(HIREDATE, 'YYYY-MM-DD') AS HIREDATE
FROM EMP
WHERE TO_CHAR(HIREDATE, 'YYYY-MM-DD') > (SELECT TO_CHAR(HIREDATE, 'YYYY-MM-DD')
                    FROM EMP
                        WHERE SAL IN (SELECT MAX(SAL)
                                                FROM EMP)
)
;
```

* * *

### QUIZ08 
**FORD 보다 입사일이 늦은 사원 중 급여가 가장 높은 사원의 이름과 급여를 출력**
```sql
SELECT
ENAME, SAL
FROM 
(SELECT ENAME, SAL
FROM EMP
WHERE HIREDATE > (SELECT HIREDATE FROM EMP WHERE ENAME = 'FORD')
ORDER BY SAL DESC)
WHERE ROWNUM = 1;
```

* * *

### QUIZ09 
**각 사원 별 시급을 계산하여 부서번호, 사원이름, 시급을 출력하시오.
조건1. 한달 근무일수는 20일, 하루 근무시간은 8시간이다.
조건2. 시급은 소수 두 번째 자리에서 반올림한다.
조건3. 부서별로 오름차순 정렬
조건4. 시급이 많은 순으로 출력.**
```sql
SELECT DEPTNO, ENAME, ROUND(SAL / (20 * 8),1) AS 시급
FROM EMP
ORDER BY DEPTNO ASC, 시급 DESC;
```

* * *

### QUIZ10
**QUIZ09에서 시급이 15이상**
```sql
SELECT T1.DEPTNO, T1.ENAME, T1.시급
FROM
(
SELECT DEPTNO, ENAME, ROUND(SAL / (20 * 8),1) AS 시급
FROM EMP
) T1
WHERE T1.시급 > 15
ORDER BY T1.DEPTNO ASC, T1.시급 DESC;
```

* * *

### QUIZ11 
**입사일로부터 지금까지 근무년수가 40년 이상인 사원의 사원번호, 사원명, 입사일, 근무년수를 출력하시오.
조건. 근무년수는 월을 기준으로 버림 (예:30.4년 = 30년, 30.7년=30년)**
```sql
SELECT S.*
FROM(
SELECT EMPNO, ENAME, HIREDATE, TRUNC((SYSDATE - HIREDATE) / 365) AS 근무년수
FROM EMP
) S
WHERE 근무년수 > 40;

-- 40년 미만

SELECT S.*
FROM(
SELECT EMPNO, ENAME, HIREDATE, TRUNC((SYSDATE - HIREDATE) / 365) AS 근무년수
FROM EMP
) S
WHERE 근무년수 < 40;
```

* * *

### QUIZ12 
**MGR가 'KING‘인 모든 사원의 이름과 급여를 출력하라.**
```sql
SELECT ENAME, SAL
FROM EMP
WHERE MGR IN
(SELECT EMPNO FROM EMP WHERE ENAME = 'KING')
;
```

* * *

### QUIZ13 
**사원 테이블에서 각 사원의 사원번호, 사원명, 매니저번호, 매니저명을 출력하시오.
조건1. 각 사원의 급여(SAL)는 매니저 급여 이상이다.**
```sql
SELECT ONE.EMPNO, ONE.ENAME, TWO.EMPNO AS MGR_NO, TWO.ENAME AS MGR_NAME
FROM EMP ONE, EMP TWO
WHERE ONE.MGR = TWO.EMPNO
AND ONE.SAL >= TWO.SAL;
```

* * *

### QUIZ14 
**커미션을 받는 사원과 (부서번호, 월급)이 같은 사원의 이름, 월급, 부서번호를 출력하시오.**
```sql
--처음에는 커미션 포함 월급이 일반 월급과 같은 사람을 구하는 것인 줄 알았다.
--처음 작성한 코드
SELECT E2.ENAME, E2.SAL, E2.DEPTNO
FROM EMP E1, EMP E2
WHERE (E1.SAL + E1.COMM) = E2.SAL;

--알고보니 커미션 받은 사원을 구하는 쿼리를 작성하는 것이었다.
-- 커미션 받은 사람
SELECT ENAME, SAL, DEPTNO
FROM EMP
WHERE (DEPTNO, SAL) IN (SELECT DEPTNO, SAL FROM EMP
WHERE COMM IS NOT NULL AND COMM > 0);
```




