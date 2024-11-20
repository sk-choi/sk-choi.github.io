---
title : "데이터베이스 SQL 구문 기초06(GROUP BY, HAING, 그룹 함수)"
date : 2024-11-20 16:32:30 +/-TTTT
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

### 그룹(GROUP)

GROUP BY

조건 : HAVING

WHERE는 SELECT문에 대한 조건

* * *

### 그룹 함수

개수: COUNT(\*), COUNT(1), COUNT(컬럼)

평균: AVG(컬럼)

최대 최소: MAX(컬럼) MIN(컬럼)

총합: SUM(컬럼)

**COUNT()**

```sql
SELECT COUNT(*), COUNT(1), COUNT(EMPNO), COUNT(COMM)

FROM EMP;
```

COUNT(컬럼)을 실행하면 결측값 NULL은 없는 취급한다.(NULL값 제외한 개수 출력)

COUNT(\*)은 레코드 전체를 읽는다. -> 비효율적

COUNT(1) : 고유의 레코드 번호를 읽는다.(ROWID)

ROWID: 파일 블럭, 블럭 ID 존재->변경X

ROWNUM: INSERT시 들어온 순서

오라클 어딘가에 쿼리 실행 기록이 남아있다.

대용량 DB의 경우 COUNT(1)이 더 효율적

**AVG()**

```sql
SELECT AVG(SAL)

FROM EMP;
```

평균 무조건 AVG 아님

업무 성격에 따라 다르다.

**CEIL()**

CEIL : 올림

```sql
SELECT CEIL(1.1) FROM DUAL;
```

2출력

**FLOOR()**

FLOOR : 내림

```sql
SELECT FLOOR(1.1) FROM DUAL;
```

1출력

**TRUNC()**

TRUNC(값, 버려서 보여질 자릿수) 디폴트는 전부

EX. TRUNC(1.1234, 1)

```sql
SELECT TRUNC(1.1234) FROM DUAL;
```

1.1 출력

**MAX(), MIN()**

날짜에는 최소 날짜 최대 날짜

글자에는 앞글자 뒷글자

* * *

### 그룹 함수 관련 에러 케이스

&nbsp;

**케이스 01**

```sql
SELECT D.DEPTNO, D.DNAME, MIN(E.SAL) AS MSAL, MAX(E.SAL) AS XSAL

FROM EMP E, DEPT D

WHERE E.DEPTNO = D.DEPTNO

GROUP BY D.DEPTNO;
```

이 쿼리를 실행하면

에러번호: `ORA-00979 GROUP BY EXPESSION`오류 발생

그룹함수와 일반 컬럼을 동시에 출력할 경우

반드시 일반 컬럼을 그룹바이에도 넣으라는 것.

일반 컬럼과 그룹함수를 동시에 SELECT할 때에는

GROUP BY 함수에도 일반 컬럼이 모두 들어가야 한다.(강조)

**수정한 쿼리**

```sql
SELECT D.DEPTNO, D.DNAME, MIN(E.SAL) AS MSAL, MAX(E.SAL) AS XSAL

FROM EMP E, DEPT D

WHERE E.DEPTNO = D.DEPTNO

GROUP BY D.DEPTNO, D.DNAME;
```

&nbsp;

**케이스 02**

```sql
SELECT DEPTNO, SUM(SAL)

FROM EMP
```

에러번호: ORA-00937 not single-group group function

GROUP BY 없어서 에러

**수정한 쿼리**

```sql
SELECT DEPTNO, SUM(SAL)

FROM EMP

GROUP BY DEPTNO;
```

&nbsp;

**케이스 03**

```sql
SELECT DEPTNO, SUM(SAL)

FROM EMP

WHERE SUM(SAL) > 9000

GROUP BY DEPTNO;
```

에러번호: ORA-00934: group function is not allowed here

그룹 함수를 where절에서 조건으로 사용하기 때문에 오류 발생

**수정한 쿼리**

```sql
SELECT DEPTNO, SUM(SAL)

FROM EMP

GROUP BY DEPTNO

HAVING SUM(SAL) > 9000;
```

&nbsp;

그룹함수는 그룹 바이 없어도 사용 가능

그룹 바이가 없을 경우엔 테이블 전체를 대상으로 그룹짓는다.

\==> 1건의 결과 레코드가 나옴

&nbsp;

**케이스 04**

```sql
SELECT DEPTNO, MAX(SUM(SAL))

FROM EMP

GROUP BY DEPTNO;
```

에러 발생: ORA-00937

GROUP BY로는 DEPTNO까지 출력할 방법 없다.

해결하기 위해선 서브 쿼리를 작성해야 한다.

* * *

### 퀴즈

**각 부서별 직업별 급여 총합 출력**

```sql
SELECT DEPTNO, JOB, SUM(SAL) AS SSAL

FROM EMP

GROUP BY DEPTNO, JOB

ORDER BY DEPTNO ASC, JOB ASC;
```

&nbsp;

&nbsp;