---
title : "[Spring] 스프링 MVC 정리"
date : 2025-03-02 01:14:30 +/-TTTT
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

이 글은 스프링의 MVC 패턴에 대해 정리한 글입니다.

---

### **스프링 MVC 동작 원리**

<img src="https://velog.velcdn.com/images/shinyounseob/post/2a01c80e-95fe-4f6a-8121-9f54f3c20811/image.png" alt="img" width="564" height="267" class="jop-noMdConv">

### DispatcherServlet

- 스프링 MVC의 중심이자 진입점.
- 모든 요청을 받아 적절한 컴포넌트로 전달하고 최종적으로 응답을 생성.
- 서블릿 컨테이너와 스프링 컨테이너 간의 중추 역할을 담당.

* * *

### **스프링 MVC의 주요 컴포넌트**

#### 1\. ContextLoaderListener와 `root-context.xml`

- 애플리케이션 전역에서 사용할 빈(bean)을 설정.
- 비즈니스 로직, 데이터베이스 관련 설정 포함.

#### 2\. DispatcherServlet과 `servlet-context.xml`

- 웹과 관련된 빈(bean)을 관리.
- 컨트롤러, 뷰 리졸버(ViewResolver), 핸들러 맵핑(HandlerMapping) 포함.

#### 3\. Controller와 @Controller

- 사용자의 요청을 처리하는 핵심 역할.
- 요청을 처리한 후 결과를 View 또는 데이터를 반환.

#### 4\. HandlerMapping (@RequestMapping)

- 요청 URL과 컨트롤러 메서드를 매핑.
- 예: `/hello` 요청 → `home()` 메서드 실행.

#### 5\. ViewResolver

- 컨트롤러의 반환값(View 이름)을 기반으로 실제 View 파일 경로 결정.
- 기본 조합: `prefix + 메서드 리턴 값 + suffix`
    - 예: `/WEB-INF/views/` + `home` + `.jsp` → `/WEB-INF/views/home.jsp`

* * *

### **스프링 MVC의 요청 흐름**

1.  **클라이언트 요청** → DispatcherServlet으로 전달.
2.  DispatcherServlet이 **HandlerMapping**을 통해 요청에 맞는 컨트롤러를 찾음.
3.  컨트롤러가 **서비스 계층(Service Layer)** 호출.
4.  서비스 계층이 DAO와 협력하여 데이터베이스와 상호작용.
5.  컨트롤러는 서비스 계층의 결과를 View 이름으로 DispatcherServlet에 반환.
6.  DispatcherServlet이 **ViewResolver**를 사용해 최종 View를 찾고 렌더링.
7.  클라이언트에 응답.

* * *

### **서비스 계층(Service Layer)의 역할과 중요성**

#### 서비스 계층 없이 구성할 경우의 문제점

1.  **컨트롤러의 비대화**: 비즈니스 로직이 섞여 복잡도가 증가.
2.  **재사용성 부족**: 동일 로직이 여러 컨트롤러에 중복.
3.  **테스트 어려움**: 컨트롤러가 로직을 포함하면 독립적 테스트가 어려움.

#### 서비스 계층의 주요 기능

1.  **비즈니스 로직 처리**: 데이터 처리와 비즈니스 규칙을 캡슐화.
2.  **트랜잭션 관리**: 여러 DAO 작업을 하나의 논리적 단위로 묶어 일관성 유지.
3.  **컨트롤러와 DAO 간의 중재자 역할**.

#### 컨트롤러는 초경량화가 원칙

- 단순히 요청을 받아 서비스를 호출하고 결과를 반환.
- 복잡한 조건문, 프로세스는 서비스 계층에 위임.

* * *

### **스프링의 리턴과 모델 처리**

#### 리턴 방식

1.  기본 리턴: View 이름으로 처리.
2.  `redirect:`: 리다이렉트.
3.  `forward:`: 특정 경로로 포워딩.

#### Model

- 데이터를 전달하기 위해 스프링이 제공하는 객체.
- request 객체를 직접 사용하지 않고 Model로 데이터를 넘김.

#### @RequestParam

- HTTP 요청 파라미터를 메서드 매개변수에 바인딩.
- 기본적으로 String으로 처리되며, 선언된 자료형에 맞게 자동 변환.

* * *

### **스프링의 주요 장점**

1.  **추상화와 다형성 (0순위)**
    
    - 인터페이스를 활용해 유연한 구현 변경 가능.
    - 비즈니스 로직과 기술적 세부 사항 분리.
2.  **비즈니스 로직 중심 (1순위)**
    
    - 서비스 계층에 로직 집중, 유지보수 용이.
3.  **보안 (2순위)**
    
    - 스프링 시큐리티로 인증 및 권한 관리.

* * *

### **비유: 협상 프로세스**

1.  **클라이언트 요청 → 전달병(컨트롤러)**
    
    - 사용자의 요청을 받아 처리할 준비.
2.  **전달병 → 사신단(서비스 계층)**
    
    - 비즈니스 로직과 관련된 작업을 수행하며 데이터를 요청하거나 업데이트.
3.  **사신단 → 행정본부(DAO)**
    
    - 데이터베이스와 직접 상호작용.
4.  **행정본부 → 사신단 → 전달병**
    
    - 결과를 되돌려받아 클라이언트에 응답.

