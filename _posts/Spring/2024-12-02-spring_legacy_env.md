---
title : "[Spring Legacy] 스프링 레거시 개발 환경 구축"
date : 2024-12-02 00:26:30 +/-TTTT
categories : 
- Spring
tags : 
- [spring] #소문자만 가능
published : true
#permalink : categories/data_structure

#header :
  #teaser : 

toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
---
### Intro

kosta 강의 중에서 설치한 스프링 레거시 환경 구축 방법에 대한 정보를 정리한 글이다.

* * *
### 설치할 파일

**1\. 톰캣설치**  
C:\\IT(이하 IT) 폴더 밑에 압축풀기  
설치파일 : apache-tomcat-9.0.86-windows-x64.zip  
설치완료 : C:\\IT\\apache-tomcat-9.0.86  

**2\. jstl-1.2.jar 설치**  
C:\\IT\\apache-tomcat-9.0.86\\lib안에 카피

**3\. IDE 설치 : STS3(v 3.9.17)**  
압축풀어 sts-bundle> sts-3.9.17.RELEASE 폴더를 C:\\IT밑에 카피  
sts-3.9.17.RELEASE 폴더명을 "STS3917"로 변경  
설치파일 : S3917_J11.zip  
설치완료 : C:\\IT\\STS3917

* * *

### 수정 사항
**1) STS.ini 설정 수정** 
openFile 이하 라인에 vm 추가  
\-vm  
C:\\IT\\jdk-11.0.22\\bin\\javaw.exe

**2) 최소 메모리 크기 늘리기**  
\-Xms1024m

**3) workspace, repository 폴더 생성**  
C:\\IT\\STS3917\\STS3_WORKSPACE : 코드  
C:\\IT\\STS3917\\STS3_REPOSITORY : maven 빌드,배포

**4) IDE 실행 시 미리 만들어놓은 workspace 위치 지정**  
C:\\IT\\STS3917\\STS3_WORKSPACE

* * *

### IDE 셋팅 수정 (Winodw > Preferences 에서 변경)
**1) mvn repositoy 위치 변경**  
: Maven > User Settings > 2번째Browse버튼 클릭> sts3_mvn_setting.xml 지정  
C://IT//STS3917//STS3_REPOSITORY 위치로 변경확인

**2) 인코딩 (UTF-8) 포맷 변경**  
preference 좌측 상단 검색창에 encoding 입력 후 나온 결과 트리 모두 변경  
\- Content Types : Default encoding UTF-8 입력 후 Update버튼클릭  
\- Workspace : Text file encoding UTF-8 변경 후 Apply버튼클릭  
\- CCS Files : Encoding UTF-8 변경 후 Apply버튼클릭  
\- HTML Files : Encoding UTF-8 변경 후 Apply버튼클릭  
\- JSP Files : Encoding UTF-8 변경 후 Apply버튼클릭

**3) 콘솔 리밋 버퍼 해제**  
Run/Debug > Console > Limit console output 체크박스 해제 Apply버튼클릭

**4) 내부브라우저 > chrome으로 변경**
Windows > Preference > Browse > Web Browser >  
\> 상단 Use extenal web browser 체크 > 하단 Chrome 체크 > Apply버튼클릭

* * * 

### JAVA 프로젝트 생성 및 구동 테스트  
File > New > Java Project  
Project name : prj_java  
Hello.java 클래스 생성 후 구동 테스트

* * *

### DynamicWeb 프로젝트 생성 및 구동 테스트  
**1) 프로젝트 생성**  
File > New > Other > Web > Danamic Web Project  
Project name : prj_web  
Target runtime : NewRuntime버튼 클릭 > Apache Tomecat 9.0 > Next버튼 > Browse버튼  
\> C:\\IT\\apache-tomcat-9.0.86 위치 지정 > Finish버튼  
\> Next > Next > Generate web.xml 체크박스 체크 > Fish버튼

**2) WAS 연동**  
프로젝트선택 > 우클릭 > Run As > Run on Server > Apache Tomecat 9.0선택 >  
Server Name : WEB SERVER (선택적으로 알아볼 수 있는 이름으로 변경)  
Always use this....... 체크박스 체크  
\> Next > Finish  
\-- --------------------------------------------------------  
\--Port 8080... already in use(에러발생)  
\-- Servers탭 클릭 (없으면 Windows > ShowView에서 꺼내기)  
\> WEB SERVERS 더블클릭 > port번호 변경 후 Ctrl+S > 서버재실행

**3) 코드 생성 후 브라우저 테스트**  
\[Front\] http://localhost:8081/prj_web/NewFile.jsp  
src>main>webapp > 마우스우클릭 > New > Other(JSP File) > .. Finish  
NewFile.jsp > 마우스우클릭 > Run As > Run on Server

\[Backend\] http://localhost:8081/prj_web/HelloServlet  
src>main>java > 마우스우클릭 > New > Other(Servlet File) > .  
Java Package : com.test.pkg  
Class name : HelloServlet  
\> Finish버튼 클릭

* * * 

### SpringFramework IDE (Legacy v.4.x)  
1) 설치완료 : C:\\IT\\STS_SPRING  
2) STS.ini 설정 수정 (3번 항목과 동일) : vm memory변경  
3) workspace, repository 폴더 생성  
C:\\IT\\STS_SPRING\\SPRING_WORKSPACE : 코드  
C:\\IT\\STS_SPRING\\SPRING_REPOSITORY : maven 빌드,배포  
4) IDE 실행 시 미리 만들어놓은 workspace 위치 지정  
C:\\IT\\STS_SPRING\\SPRING_WORKSPACE  
5) 4. IDE 셋팅 수정 항목과 동일

* * *

### Spring Legacy 프로젝트 생성 및 테스트  
1) C:\\IT\\STS_SPRING\\SPRING_WORKSPACE\\.metadata\\.plugins\\org.springsource.ide.eclipse.commons.content.core  
https-content.xml 카피 후 STS 재실행 

2) Spring Legacy Project > 프로젝트명:prj_spring ------ STS 재실행

3) C:\\IT\\STS_SPRING\\SPRING_WORKSPACE\\.metadata\\.sts\\content\\org.springframework.templates.mvc-3.2.2  
org.springframework.templates.mvc-3.2.2.zip 압축풀기  
3개의 압축파일 보이면 성공 ------ STS 재실행  

4) WAS 연동 (6-2항목 동일)
