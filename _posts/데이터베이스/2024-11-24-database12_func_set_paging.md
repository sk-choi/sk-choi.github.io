---
title : "데이터베이스 SQL 구문 기초12(오라클 기타 함수와 집합, 페이징 기법)"
date : 2024-11-24 11:09:30 +/-TTTT
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

**오라클 통계 / 집계 / 분석 함수**

### RANK()
**RANK() OVER ORDER BY 옵션**

동등한 순위가 있으면 그 다음 순위는 동등한 순위가 나온만큼의 횟수를 더한 순위

```sql
-- RANK() OVER (ORDER BY 컬럼 ASC / DESC) AS TT  
-- 1 2 2 4  
SELECT SAL,  
RANK() OVER (ORDER BY SAL DESC) AS RK  
FROM EMP;
```

**RANK() OVER PARTITION BY 옵션**

그룹별로 랭킹을 부여하는 함수

```sql
-- RANK()OVER (PARTITION BY 컬럼 ASC/DESC) AS PK  
-- 그룹 별로 랭킹 매길 때  
SELECT DEPTNO, SAL  
FROM EMP  
GROUP BY DEPTNO, SAL  
ORDER BY DEPTNO ASC, SAL DESC;

  
-- ^ 위 쿼리는 오류 발생

  
-- PARTITION BY를 사용한 쿼리  
SELECT DEPTNO, SAL,  
RANK()OVER (PARTITION BY DEPTNO ORDER BY SAL DESC) AS PK  
FROM EMP;
```
* * *

### DENSE_RANK()
동등한 순위가 있으면 그 다음 순위는 전 순위의 바로 다음 순위
```sql
-- DENSE_RANK() OVER (ORDER BY 컬럼 ASC / DESC) AS RK  
-- 1 2 2 3  
SELECT SAL,  
DENSE_RANK() OVER (ORDER BY SAL DESC) AS RK  
FROM EMP;
```
* * *

### ROWNUM과 ROW_NUMBER()

ROWNUM은 테이블 조회 결과마다 생성되는 번호이고, ROW_NUMBER() 함수는 이러한 ROWNUM 기준으로 순위를 부여한다.

```sql
-- ROW NUM -- INSERT한 순서로 번호 매겨짐  
SELECT ROWNUM, SAL, ENAME  
FROM EMP  
ORDER BY SAL DESC;

--여기서 ROWNUM은 인라인뷰의 ROWNUM  
SELECT ROWNUM, RNUM, SAL, ENAME  
FROM (  
SELECT ROWNUM AS RNUM, SAL, ENAME  
FROM EMP  
ORDER BY SAL DESC  
);  
--WHERE ROWNUM BETWEEN 1 AND 3;  
-- ^ 이건 오라클 환경에서 LIMIT 기법을 사용하는 방법이기도 하다.

--ROW_NUMBER() OVER (ORDER BY 컬럼 ASC / DESC) RN  
SELECT SAL, ROWNUM,  
ROW_NUMBER() OVER (ORDER BY SAL DESC) AS RN  
FROM EMP;
```
* * *

### 집합 

집합(ANSI 표준이라 어디서든 사용 가능)  
1\.일반 합집합: UNION

2\.정렬 없는 합집합: UNION ALL

3\. 교집합: INTERSECT 4.차집합: MINUS

**집합 연산 테스트를 위한 테이블 생성(A,B)**

```sql
CREATE TABLE A(  
NUM NUMBER, GUBUN VARCHAR(1));

CREATE TABLE B(  
NUM NUMBER, GUBUN VARCHAR(1));

INSERT INTO A VALUES(1,'A');  
INSERT INTO A VALUES(2,'A');  
INSERT INTO A VALUES(3,'A');

INSERT INTO B VALUES(3,'B');  
INSERT INTO B VALUES(4,'B');  
INSERT INTO B VALUES(5,'B');

SELECT * FROM A;  
SELECT * FROM B;
```

**합집합**
```sql
--UNION: 중복 제거  
SELECT NUM FROM A  
UNION  
SELECT NUM FROM B;

--UNION ALL: 중복 포함  
SELECT NUM FROM A  
UNION ALL  
SELECT NUM FROM B;
```
**교집합: INTERSECT**
```sql  
SELECT NUM FROM A  
INTERSECT  
SELECT NUM FROM B;
```
**차집합**
```sql
-- A-B  
SELECT NUM FROM A  
MINUS  
SELECT NUM FROM B;

-- B-A  
SELECT NUM FROM B  
MINUS  
SELECT NUM FROM A;
```
* * *

### 페이징

데이터베이스의 조회 결과를 원하는 범위만큼 출력하는 것.

```sql
SELECT ROWNUM, SS.*  
FROM  
(  
SELECT ROWNUM AS ORDER_NUM, S.\*  
FROM (  
SELECT ROWNUM AS INIT_RNUM, ENAME, HIREDATE  
FROM EMP  
ORDER BY HIREDATE DESC  
)S  
)SS  
WHERE ORDER_NUM BETWEEN 5 AND 10;  
-- ROWNUM은 TOP부터 출력 가능, 중간부터 출력은 불가능  
-- ROWNUM은 레코드 출력 시 생성되는 번호라서 중간부터 출력할려면 서브쿼리로 3번 감싸야 한다.  
-- MARIA, MYSQL은 LIMIT을 사용하면 된다.
```

**더 간단히 하는 방법(ROW_NUMBER() 이용)** 
```sql
SELECT S.*  
FROM(  
SELECT ENAME, HIREDATE, ROW_NUMBER() OVER (ORDER BY HIREDATE DESC) AS RK  
FROM EMP  
)S  
WHERE RK BETWEEN 5 AND 10;
```