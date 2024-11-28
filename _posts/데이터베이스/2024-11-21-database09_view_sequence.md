---
title : "데이터베이스 SQL 구문 기초09(VIEW, SEQUENCE)"
date : 2024-11-21 21:38:30 +/-TTTT
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

### 뷰(VIEW)

원본은 있는데 가상으로 테이블을 만드는 것

원본 데이터는 저장소에 따로 있고

뷰는 그 데이터에 대한 링크만 가지고 있다.

**뷰 생성**

```sql
CREATE VIEW 뷰이름 AS SELECT ~ FROM ~ WHERE
```

**예시**

```sql
CREATE VIEW MY_VIEW AS 

SELECT E.EMPNO, E.JOB, E.SAL, D.DNAME

FROM EMP E, DEPT D WHERE E.DEPTNO = D.DEPTNO

AND JOB IN (SELECT JOB FROM EMP WHERE DEPTNO = 10);
```

&nbsp;

뷰는 삭제하면 테이블도 삭제 된다.

그래서 뷰를 줄 때 READ ONLY 권한만 줄 수 있다. (SELECT만 가능)

복잡한 쿼리를 조인한 결과를 미리 만들어서 여러가지를 해 볼 수 있게 함 (부하 감소)

보안목적: 회원테이블 같은 민감한 정보가 있는 테이블을 가공해야 할 때

관리나 보안의 목적으로 일부 데이터를 가린 뷰를 사용할 수 있다.

메모리 효율성

&nbsp;

**이미 뷰를 만들었는데 컬럼 추가를 해야 할 경우**

```sql
CREATE OR REPLACE VIEW MY_VIEW AS

SELECT EMPNO, ENAME, JOB FROM EMP;
```

&nbsp;

**뷰 삭제**

```sql
DROP VIEW 뷰이름
```

&nbsp;

**단순 뷰 :** 테이블 하나로 만드는 것.

**복합 뷰 :** 여러 테이블로 만드는 것.

**인라인 뷰 :** FROM절에서 참조하는 테이블의 크기가 클 경우, 서브쿼리를 통해 임시적으로 사용할 테이블을 사용하는 것. 임시성 테이블이라서 1회성이다.

* * *

### 시퀀스(SEQUENCE)

내가 번호를 발행하는 순간 번호가 계속 나온다.

시퀀스를 drop하는 순간 다시 1번부터 나온다.

레코드를 구분할 수 있는 무언가를 만들 때 필요

시퀀스 하나로 여러 테이블이 같이 써도 되지만,

현업에서는 테이블 당 한개씩 사용한다.

**시퀀스 만들기**

```sql
CREATE SQUENCE 시퀀스 이름 
STARTWITH 1
INCREMENT BY 1
MINVALUE 1
NOCYCLE;
```

**시퀀스 변경**

```sql
ALTER SEQUENCE 시퀀스명 
INCREMENT BY 1
MINVALUE 1
NOCYCLE;
```

근데 START WITH는 변경 불가능

시퀀스는 PK에 사용할 수 있는데,  
START WITH를 수정하면 중복되거나 충돌할 수 있어서 그렇다.

**시퀀스 삭제**

```sql
DROP SEQUENCE 시퀀스명
```

&nbsp;

**시퀀스의 값을 증가시키는 법**
```sql
SELECT SEQ_BOARD.NEXTVAL FROM 테이블명;
```
&nbsp;

**현재 시퀀스의 값을 알아내는 법**
```sql
SELECT SEQ_BOARD.CURRVAL FROM 테이블명;
```
