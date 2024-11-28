---
title : "데이터베이스 SQL 구문 기초05(OUTER JOIN, ANSI JOIN과 비교)"
date : 2024-11-20 16:24:30 +/-TTTT
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

### OUTER JOIN

**OUTER JOIN 사용 예시**

**예시1.**

댓글 시스템에서 원본 글의 게시글 번호를 댓글 데이터베이스가 참조할 수 있도록 만든다.

원본글 데이터베이스에서는 글 번호가 PK, 댓글 데이터베이스에서는 글 번호가 FK

근데 댓글 데이터베이스와 원본글 데이터베이스를 EQUI JOIN 했을 때

댓글이 없는 경우에는 글이 안 보일 수 있는 문제점이 발생

이때 OUTER JOIN을 사용한다.

**예시2.**

상품 테이블과 주문 테이블이 존재할 때 주문 현황 테이블을 조인을 통해 만든다면

동등 조인을 사용한다면 주문하지 않은 상품의 정보는 아예 출력되지 않는다.

주문 안 한 것도 출력하기 위해서 OUTER JOIN을 사용한다.

* * *

### ORACLE JOIN 연산자(+)

```sql
SELECT E.ENAME, D.DEPTNO

FROM EMP E, DEPT D

WHERE E.DEPTNO (+) = D.DEPTNO

ORDER BY D.DEPTNO ASC;
```

&nbsp;

아래의 쿼리도 같은 결과가 나온다.

```sql
SELECT E.ENAME, D.DEPTNO

FROM EMP E, DEPT D

WHERE D.DEPTNO = E.DEPTNO(+)

ORDER BY D.DEPTNO ASC;
```

&nbsp;  
기준이 되는 데이터를 뽑는다. (PK기준)

* * *

### NON EQUI JOIN

등호가 아닌 연산자를 사용한 JOIN을 NON EQUI JOIN이라고 한다.

```sql
SELECT E.NAME, E.SAL, S.GRADE, S.LOSAL, S.HISAL

FROM EMP E, SALGRADE S

WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL
```

&nbsp;

비교 연산자로는 아래와 같이 작성이 가능하다.

```sql
SELECT E.NAME, E.SAL, S.GRADE, S.LOSAL, S.HISAL

FROM EMP E, SALGRADE S

WHERE E.SAL >= S.LOSAL AND E.SAL <= S.HISAL;
```

* * *

### ANSI JOIN

ANSI표준에 맞는 조인 표기법.

(+)표기를 사용한 방법과 표현이 다르다.

**JOIN**

```sql
SELECT E.EMPNO, E.ENAME,D.LOC,D.DEPTNO

FROM EMP E, DEPT D

WHERE E.DEPTNO = D.DEPTNO;
```

**ANSI JOIN**

```sql
SELECT E.EMPNO, E.ENAME, D.LOC, D.DEPTNO

FROM EMP E JOIN DEPT D ON E.DEPTNO = D.DEPTO
```

디폴트가 INNER JOIN이다.

* * *

### QUIZ

출력할 컬럼: DEPTNO, DNAME, EMPNO, ENAME, SAL, GRADE

조건: 부서에 사원이 한명도 없어도 부서 정보 출력

**(+)표기를 사용한 방법**

```sql
SELECT D.DEPTNO, D.DNAME, E.EMPNO, E.ENAME, E.SAL, S.GRADE

FROM EMP E, DEPT D, SALGRADE S

WHERE D.DEPTNO = E.DEPTNO(+)

AND

E.SAL BETWEEN S.LOSAL(+) AND S.HISAL(+)

ORDER BY D.DEPTNO ASC;
```

(+)= 기호가 하나의 기호인지 알았는데 테이블 옆에 (+)를 붙일 수 있다는 걸 처음 알았다.

**ANSI JOIN을 사용한 방법**

1\. FULL OUTER JOIN 사용

```sql
SELECT D.DEPTNO, D.DNAME, E.EMPNO, E.ENAME, E.SAL, S.GRADE

FROM EMP E FULL OUTER JOIN DEPT D ON E.DEPTNO = D.DEPTNO

FULL OUTER JOIN SALGRADE S ON E.SAL BETWEEN S.LOSAL AND S.HISAL
```

2\. LEFT OUTER JOIN 사용

```sql
SELECT D.DEPTNO, D.DNAME, E.EMPNO, E.ENAME, E.SAL, S.GRADE

FROM DEPT D LEFT OUTER JOIN EMP E ON D.DEPTNO = E.DEPTNO

LEFT OUTER JOIN SALGRADE S ON E.SAL BETWEEN S.LOSAL AND S.HISAL
```

&nbsp;