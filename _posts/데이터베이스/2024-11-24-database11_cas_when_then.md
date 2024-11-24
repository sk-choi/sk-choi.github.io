---
title : "데이터베이스 SQL 구문 기초11(CASE WHEN THEN)"
date : 2024-11-24 11:05:30 +/-TTTT
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

CASE WHEN THEN: 범윗값 비교에 사용한다.

EX.

```sql
CASE WHEN THEN '값1'  
\[ELSE\] '값2'
```

CASE 절이 컬럼 이름이 되기 때문에 ALIAS 사용할 것

**QUIZ01. EMP 테이블에서 급여가 2001이상 3000이하면 'A' 급여가 3001에서 4000 'B'**  
**나머지 'C' 로 사원 정보(직원 번호, 직원 이름, 급여)와 같이 출력해라.**

```sql
SELECT EMPNO, ENAME, SAL,  
CASE  
WHEN (SAL BETWEEN 2001 AND 3000) THEN 'A'  
WHEN (SAL BETWEEN 3001 AND 4000) THEN 'B'  
ELSE 'C'  
END AS GRADE_NEW  
FROM EMP;
```

또는

```sql
SELECT EMPNO, ENAME, SAL,  
CASE  
WHEN (SAL >= 2001 AND SAL <= 3000) THEN 'A'  
WHEN (SAL >= 3001 AND SAL <= 4000) THEN 'B'  
ELSE 'C'  
END AS GRADE_NEW  
FROM EMP;
```

**주의 사항**

CASE문에서 특정 범위에 대한 처리가 없을 경우 NULL값이 생성됨.

**QUIZ02. 부서 번호 한글로 변환**

```sql
SELECT DEPTNO,  
CASE  
WHEN (DEPTNO = 10) THEN '십'  
WHEN (DEPTNO = 20) THEN '이십'  
WHEN (DEPTNO = 30) THEN '삼십'  
ELSE '기타 부서'  
END AS KOR_DEPTNO  
FROM EMP;
```

DECODE 사용

```sql
SELECT DEPTNO,  
DECODE(DEPTNO, 10, '십',  
20, '이십',  
'삼십'  
)  
AS KOR_DEPTNO  
FROM EMP;
```

이 코드가 더 예외 사항을 잘 처리한다!

```sql
SELECT DEPTNO,  
DECODE(DEPTNO, 10, '십',  
20, '이십',  
30, '삼십',  
기타부서)  
AS KOR_DEPTNO  
FROM EMP;
```

이중함수도 가능?? -> 불가능

```sql
SELECT DEPTNO,  
DECODE(DEPTNO, 10, '십', (DEPTNO, 20, '이십', (DEPTNO, 30, '삼십')))  
AS KOR_DEPTNO  
FROM EMP;
```

  

**QUIZ03. COMM NULL 이면 0, COMM있으면 그대로 출력**

```sql
SELECT COMM,  
CASE  
WHEN (COMM IS NULL) THEN 0  
WHEN (COMM IS NOT NULL) THEN NVL(COMM,0)  
END AS TR_NULL_0  
FROM EMP;
```

아래 버전이 더 간단하다.

```sql
SELECT COMM,  
CASE  
WHEN (COMM IS NULL) THEN 0  
ELSE COMM  
END AS TR_NULL_0  
FROM EMP;
```

DECODE 사용한 버전

```sql
SELECT COMM,  
DECODE(COMM, NULL, 0, COMM)  
AS TR_NULL_0  
FROM EMP;
```