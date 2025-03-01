---
title : "[Java] MariaDB와 연결하기"
date : 2025-03-02 00:42:30 +/-TTTT
categories : 
- Java
tags : 
- [Java] #소문자만 가능
published : true
#permalink : categories/algorithm

header :
  teaser : https://blog.kakaocdn.net/dn/ehfQWK/btrnP7Cexxc/ZmLpToeisMobjHGaLfEDg0/img.png

toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
---

### Intro

이 글은 자바로 MariaDB의 데이터를 가져오는 코드를 리뷰하는 글입니다.

* * *

### 필요한 환경

MariaDB java Client 3.3.3이 필요하다 ([여기](https://mvnrepository.com/artifact/org.mariadb.jdbc/mariadb-java-client/3.3.3)에서 다운 받을 수 있다).

다운 받은  JAR 파일을 프로젝트 파일의 properties -> Java Build Path -> Libraries -> Classpath -> Add External JARs로 등록하면 된다.

이 밖의 환경 : 이클립스

* * *

### 테이블 만들기

```sql
-- DEPT 테이블 생성
CREATE TABLE DEPT (
    DEPTNO INT PRIMARY KEY,
    DNAME VARCHAR(14),
    LOC VARCHAR(13)
);

-- EMP 테이블 생성
CREATE TABLE EMP (
    EMPNO INT PRIMARY KEY,
    ENAME VARCHAR(10),
    JOB VARCHAR(9),
    MGR INT,
    HIREDATE DATE,
    SAL DECIMAL(7,2),
    COMM DECIMAL(7,2),
    DEPTNO INT,
    FOREIGN KEY (DEPTNO) REFERENCES DEPT(DEPTNO)
);

-- SALGRADE 테이블 생성
CREATE TABLE SALGRADE (
    GRADE INT,
    LOSAL INT,
    HISAL INT
);

-- DEPT 데이터 삽입
INSERT INTO DEPT (DEPTNO, DNAME, LOC) VALUES (10, 'ACCOUNTING', 'NEW YORK');
INSERT INTO DEPT (DEPTNO, DNAME, LOC) VALUES (20, 'RESEARCH', 'DALLAS');
INSERT INTO DEPT (DEPTNO, DNAME, LOC) VALUES (30, 'SALES', 'CHICAGO');
INSERT INTO DEPT (DEPTNO, DNAME, LOC) VALUES (40, 'OPERATIONS', 'BOSTON');

-- EMP 데이터 삽입
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7369, 'SMITH', 'CLERK', 7902, '1980-12-17', 800, NULL, 20);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7499, 'ALLEN', 'SALESMAN', 7698, '1981-02-20', 1600, 300, 30);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7521, 'WARD', 'SALESMAN', 7698, '1981-02-22', 1250, 500, 30);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7566, 'JONES', 'MANAGER', 7839, '1981-04-02', 2975, NULL, 20);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7654, 'MARTIN', 'SALESMAN', 7698, '1981-09-28', 1250, 1400, 30);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7698, 'BLAKE', 'MANAGER', 7839, '1981-05-01', 2850, NULL, 30);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7782, 'CLARK', 'MANAGER', 7839, '1981-06-09', 2450, NULL, 10);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7788, 'SCOTT', 'ANALYST', 7566, '1987-07-13', 3000, NULL, 20);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7839, 'KING', 'PRESIDENT', NULL, '1981-11-17', 5000, NULL, 10);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7844, 'TURNER', 'SALESMAN', 7698, '1981-09-08', 1500, 0, 30);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7876, 'ADAMS', 'CLERK', 7788, '1987-07-13', 1100, NULL, 20);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7900, 'JAMES', 'CLERK', 7698, '1981-12-03', 950, NULL, 30);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7902, 'FORD', 'ANALYST', 7566, '1981-12-03', 3000, NULL, 20);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES
(7934, 'MILLER', 'CLERK', 7782, '1982-01-23', 1300, NULL, 10);

-- SALGRADE 데이터 삽입
INSERT INTO SALGRADE (GRADE, LOSAL, HISAL) VALUES (1, 700, 1200);
INSERT INTO SALGRADE (GRADE, LOSAL, HISAL) VALUES (2, 1201, 1400);
INSERT INTO SALGRADE (GRADE, LOSAL, HISAL) VALUES (3, 1401, 2000);
INSERT INTO SALGRADE (GRADE, LOSAL, HISAL) VALUES (4, 2001, 3000);
INSERT INTO SALGRADE (GRADE, LOSAL, HISAL) VALUES (5, 3001, 9999);

COMMIT;

```

&nbsp;

### DBmanager 인터페이스 만들기

```java
package mariadb;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public interface DBmanager {
    public Connection connect();
    public void close(Connection connect, PreparedStatement pstmt, ResultSet rs);
    public void close(Connection conn, PreparedStatement pstmt);
}

```

&nbsp;

### DB_maria_connect 클래스 만들기

```java
package mariadb;

import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Properties;

public class DB_maria_connect implements DBmanager{
    
    private String DB_URL;
    private String DB_ID;
    private String DB_PW;
    
    private static DB_maria_connect instance;
    private DB_maria_connect() {}
    
    public static synchronized DB_maria_connect getInstance() {
        if (instance == null) {
            instance = new DB_maria_connect();
        }
        return instance;
    }
    
    @Override
    public Connection connect() {
        Connection conn = null;

        try {
            Class.forName("org.mariadb.jdbc.Driver");
            
            // DB 설정(properties) 읽어오기
            Properties props = new Properties();
            props.load(DB_maria_connect.class.getClassLoader().getResourceAsStream("mydb.properties"));
            DB_URL = props.getProperty("maria.url");
            DB_ID = props.getProperty("maria.id");
            DB_PW = props.getProperty("maria.pw");
            System.out.println("resources/mydb.properties...로드" + DB_URL);
            
            conn = DriverManager.getConnection(DB_URL, DB_ID, DB_PW);
            
            if (conn != null) {
                System.out.println("마리아 연결 성공");
            }
            else {
                System.out.println("마리아 연결 실패");
            }
            
        } catch (ClassNotFoundException | IOException | SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return conn;
    }
    
    
    public void close(Connection connect, PreparedStatement pstmt, ResultSet rs) {
        
        try {
            if (rs != null) rs.close();
            if (pstmt != null) pstmt.close();
            if (connect != null) connect.close();
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
    
    public void close(Connection connect, PreparedStatement pstmt) {
        try {
            if (pstmt != null) pstmt.close();
            if (connect != null) connect.close();
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

&nbsp;

### EmpVO 클래스 만들기

```java
package mariadb;

public class EmpVO {
    
    private int empno;
    private String ename;
    private int sal;
    private int deptno;
    
    public EmpVO() {};
    
    public EmpVO(int empno, String ename, int sal, int deptno) {
        this.empno = empno;
        this.ename = ename;
        this.sal = sal;
        this.deptno = deptno;
    }
    
    public int getEmpno() {
        return empno;
    }

    public void setEmpno(int empno) {
        this.empno = empno;
    }

    public String getEname() {
        return ename;
    }

    public void setEname(String ename) {
        this.ename = ename;
    }

    public int getSal() {
        return sal;
    }

    public void setSal(int sal) {
        this.sal = sal;
    }

    public int getDeptno() {
        return deptno;
    }

    public void setDeptno(int deptno) {
        this.deptno = deptno;
    }


}

```

&nbsp;

### EmpDAO 클래스 만들기

```java
package mariadb;

import java.util.*;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class EmpDAO {
    
    Connection conn = null;
    PreparedStatement pstmt = null; //sql 점검
    ResultSet rs = null; //결과 출력
    
    public ArrayList<EmpVO> select() {
    
        ArrayList<EmpVO> alist = new ArrayList<EmpVO>();
        
        DBmanager o = DB_maria_connect.getInstance(); // 다형성
        //TODO
        String sql = "select * from emp";
        try {
            conn = o.connect();
            pstmt = conn.prepareStatement(sql);
            rs = pstmt.executeQuery();
            EmpVO empvo = null;
            while(rs.next()) { // 1줄 읽는 것. (반복문으로 여러 줄 읽음)
                int i = 0;
                String ename = rs.getString("ename"); //컬럼명 넣어야 해서 "" 필수
                int empno_ = rs.getInt("empno");
                empvo = new EmpVO();
                empvo.setEmpno(empno_);
                empvo.setEname(ename);
                alist.add(empvo);
                
                System.out.println(alist.get(i));
                i++;
            }
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        o.close(conn, pstmt, rs);
        System.out.println("--내부 select(int empno) done--");
        return alist;
    }
    
    public EmpVO select(int empno) {
        
        EmpVO empvo = new EmpVO();
        
        DBmanager o = DB_maria_connect.getInstance(); // 다형성
        
        //TODO
        String sql = "select * from emp where empno=?";
        try {
            conn = o.connect();
            pstmt = conn.prepareStatement(sql);
            pstmt.setInt(1, empno);
            rs = pstmt.executeQuery();
            while(rs.next()) { // 1줄 읽는 것. (반복문으로 여러 줄 읽음)
                String ename = rs.getString("ename"); //컬럼명 넣어야 해서 "" 필수
                int empno_ = rs.getInt("empno");
                empvo.setEmpno(empno_);
                empvo.setEname(ename);
            }
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } finally {
            o.close(conn, pstmt, rs);
        }
        System.out.println("--내부 select(int empno) done--");
        return empvo;
    }
    
    public int insert(int empno, String ename, int deptno) {
        
        DBmanager o = DB_maria_connect.getInstance();
        
        //PreparedStatement pstmt = null;
        int rows = 1;
        //TODO
        String sql = "INSERT INTO EMP(EMPNO, ENAME, DEPTNO) VALUES(?,?,?)";
        
        try {
            conn = o.connect();
            pstmt = conn.prepareStatement(sql);
            pstmt.setInt(1, empno);
            pstmt.setString(2, ename);
            pstmt.setInt(3, deptno);
            rows = pstmt.executeUpdate(); //시발...
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        System.out.println("--내부 insert(int empno, String ename, int deptno) done--");
        o.close(conn, pstmt);
        return rows;
    }
    public int update(int empno, int sal) {

        DBmanager o = DB_maria_connect.getInstance();
        
        int rows = 1;
        //TODO
        String sql = "UPDATE EMP SET ENAME = 'CHANGE' WHERE EMPNO = ? AND SAL = ?";
    
        try {
            conn = o.connect();
            pstmt = conn.prepareStatement(sql);
            pstmt.setInt(1, empno);
            pstmt.setInt(2, sal);
            rows = pstmt.executeUpdate(); //시발...
        }
        catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        System.out.println("--내부 update(int empno, int sal) done--\n");
        o.close(conn, pstmt);
        return rows;
    }
    
    public int delete(int empno) {
        
        DBmanager o = DB_maria_connect.getInstance();
        
        int rows = 1;
        //TODO
        String sql = "DELETE FROM EMP WHERE EMPNO = ?";
    
        try {
            conn = o.connect();
            pstmt = conn.prepareStatement(sql);
            pstmt.setInt(1, empno);
            rows = pstmt.executeUpdate(); //시발...
        }
        catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        
        System.out.println("--내부 delete(int empno) done--\n");
        o.close(conn, pstmt);
        return rows;
    }	
    
    // --------- 호출 ---------
    public static void main(String[] args) throws InterruptedException  {
        //메서드 호출
        //return이 있을 경우 출력
        EmpDAO emp = new EmpDAO();

        EmpDAO emp = new EmpDAO();

    	EmpVO res1 = emp.select(7788);
    	System.out.println("호출결과1: " + res1.getEmpno() + "\t" +  res1.getEname());
        
    	ArrayList<EmpVO> res2 = emp.select();
    	System.out.println("호출결과2:");
    	for(int i = 0; i < res2.size(); i++) {
        	System.out.println(res2.get(i).getEmpno() + "\t" + res2.get(i).getEname());
    	}
        
    	int res3 = emp.insert(7733, "JAVA", 10);
    	System.out.println("호출결과3:" + res3 + "\n");
        
    	int res4 = emp.update(7733, 1000);
    	System.out.println("호출결과4:" + res4 + "\n");
        
    	int res5 = emp.delete(7733);
    	System.out.println("호출결과5:" + res5 + "\n");
    }
}

```

&nbsp;

**출력 결과**

> 마리아 연결 성공  
> \--내부 select(int empno) done--  
> 호출결과: mariadb.EmpVO@2a5ca609  
> resources/mydb.properties...로드jdbc:mariadb://localhost:3306/db_test  
> 마리아 연결 성공  
> \--내부 select(int empno) done--  
> 호출결과 1: 7369 SMITH  
> 호출결과 2: 7499 ALLEN  
> 호출결과 3: 7521 WARD  
> 호출결과 4: 7566 JONES  
> 호출결과 5: 7654 MARTIN  
> 호출결과 6: 7698 BLAKE  
> 호출결과 7: 7782 CLARK  
> 호출결과 8: 7788 SCOTT  
> 호출결과 9: 7839 KING  
> 호출결과 10: 7844 TURNER  
> 호출결과 11: 7876 ADAMS  
> 호출결과 12: 7900 JAMES  
> 호출결과 13: 7902 FORD  
> 호출결과 14: 7934 MILLER  
> resources/mydb.properties...로드jdbc:mariadb://localhost:3306/db_test  
> 마리아 연결 성공  
> \--내부 insert(int empno, String ename, int deptno) done--  
> 호출결과3:1
> 
> resources/mydb.properties...로드jdbc:mariadb://localhost:3306/db_test  
> 마리아 연결 성공  
> \--내부 update(int empno, int sal) done--
> 
> 호출결과4:0
> 
> resources/mydb.properties...로드jdbc:mariadb://localhost:3306/db_test  
> 마리아 연결 성공  
> \--내부 delete(int empno) done--
> 
> 호출결과5:1

&nbsp;