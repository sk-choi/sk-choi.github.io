---
title : "데이터베이스 SQL 구문 기초07(서브 쿼리, ANY, ALL)"
date : 2024-11-20 16:33:30 +/-TTTT
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

### 서브 쿼리란?

서브 쿼리 : 쿼리가 중첩(무한대로 가능)

**예시**

```sql
SELECT SAL 

FROM EMP

WHERE ENAME = 'SCOTT';
```

이 쿼리는 이름이 SCOTT인 값을 조회한다.

```sql
SELECT ENAME

FROM EMP

WHERE SAL > 3000;
```

이 쿼리는 월급이 3000인 사람을 조회한다.

```sql
SELECT ENAME

FROM EMP

WHERE SAL > (SELECT SAL FROM EMP WHERE ENAME = 'SCOTT');
```

두 쿼리를 합쳐서 이름이 SCOTT인 사람의 급여보다 큰 사람을 조회할 수 있다.

* * *

### 서브 쿼리 실행 결과가 여러 건일 경우

단일 행의 경우엔 기본 연산자를 사용할 수 있다.

다중 행의 경우엔 다음과 같은 방법을 사용할 수 있다.

&nbsp;

**QUIZ 01. 사원 번호가 7369인 사원의 직업과 같고 사원 번호가 7876인 사원의 급여보다 많은**

사람들의 이름, 급여, 직업 출력

```sql
SELECT ENAME, SAL, JOB

FROM EMP

WHERE JOB = (SELECT JOB FROM EMP WHERE EMPNO = 7369)

AND SAL > (SELECT SAL FROM EMP WHERE EMPNO = 7876);
```

&nbsp;

**QUIZ 02. 최소 급여를 받는 사람의 이름과 급여 출력**

```sql
SELECT ENAME, JOB, SAL

FROM EMP

WHERE SAL = (SELECT MIN(SAL) FROM EMP);
```

&nbsp;

**QUIZ 03. 20번 부서의 최소 급여**

```sql
SELECT DEPTNO, MIN(SAL)

FROM EMP

GROUP BY DEPTNO

HAVING DEPTNO = 20;
```

&nbsp;

**QUIZ 04. 각 부서 별 최소 급여**

```sql
SELECT DEPTNO, MIN(SAL)

FROM EMP

GROUP BY DEPTNO;
```

&nbsp;

**QUIZ 05. 각 부서의 최소 급여 중 20번 부서의 최소 급여보다 많이 받는 부서의 부서 번호, 최소 급여 출력**

```sql
SELECT DEPTNO, MIN(SAL)

FROM EMP

GROUP BY DEPTNO

HAVING MIN(SAL) > (SELECT MIN(SAL) FROM EMP GROUP BY DEPTNO HAVING DEPTNO = 20);
```

&nbsp;

**QUIZ 06. 급여가 950 또는 800 또는 1400인 직원의 이름 직업 출력**

```sql
SELECT T1.ENAME, T1.JOB

FROM (SELECT ENAME, JOB FROM EMP WHERE SAL IN (950, 800, 1400)) T1;
```

&nbsp;

**QUIZ 07. 각 부서 별로 최소 급여를 받는 사원의 이름 직업 출력**

```sql
SELECT ENAME, JOB

FROM EMP

WHERE SAL IN (SELECT MIN(SAL) FROM EMP GROUP BY DEPTNO);
```

**팁!!**

서브 쿼리는 = 보다는 IN을 사용할 것

평소엔 싱글 값이 나오던 서브 쿼리가 업데이트나 배치 작업으로 멀티 값이 나올 수 있다.

아니면 아래와 같이 서브 쿼리 멀티 컬럼 비교를 사용

**멀티 컬럼 비교**

```sql
SELECT ENAME, JOB

FROM EMP

WHERE (DEPTNO, SAL)

IN (SELECT DEPTNO, MIN(SAL) FROM EMP GROUP BY DEPTNO);  
```

&nbsp;

**QUIZ 08. 스미스 또는 마틴의 직업과 같은 직업을 갖는 사원의 이름과 직업 출력**

```sql
SELECT ENAME, JOB

FROM EMP

WHERE JOB IN (SELECT JOB FROM EMP WHERE ENAME = 'SMITH' OR ENAME = 'MARTIN');
```

&nbsp;

**QUIZ 09. 스미스와 마틴의 이름을 빼고 출력하려면**

```sql
SELECT ENAME, JOB

FROM EMP

WHERE JOB IN (SELECT JOB FROM EMP WHERE ENAME = 'SMITH' OR ENAME = 'MARTIN')

AND ENAME NOT IN ('SMITH', 'MARTIN');
```

&nbsp;

**QUIZ 10. 각 부서별 급여 총합 중 최댓값을 갖는 부서의 부서번호, 급여 총합 출력**
```sql
SELECT DEPTNO, SUM(SAL)

FROM EMP

GROUP BY DEPTNO

HAVING SUM(SAL) = (SELECT MAX(SUM(SAL)) FROM EMP GROUP BY DEPTNO);
```

!! HAVING절에서도 GROUP 함수 사용이 가능하다.
이 점을 몰라서 문제 푸는데 애를 먹었다.

* * *

### ANY & ALL

**QUIZ 11. 직업이 CLERK인 사람들의 급여보다 적게 받는 사람**

```sql
SELEC EMPNO, ENAME, JOB, SAL

FROM EMP

WHERE SAL < ANY (SELECT SAL FROM EMP WHERE JOB = 'CLERK');
```

**< ANY :** 리스트 중에 있는 값 하나 보다 작은 (최댓값보다 작은), 각각의 값을 비교해 어느 하나라도 만족하면 TRUE

**\> ALL :** 리스트 중에 있는 모든 값보다 큰(최댓값보다 큰), 최대값보다 커야 한다. 모든 값 비교해 모두 만족하면 TRUE

**\> ANY:** 최솟값보다 큰

**< ANY:** 최솟값보다 작은