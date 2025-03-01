---
title : "[Spring Legacy] 스프링 컨테이너와 일반 웹 프로젝트 비교: 의존성과 호환성"
date : 2025-03-02 01:12:30 +/-TTTT
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
이 글은 스프링과 일반 웹 프로젝트를 비교한 글입니다.

---

### 1. 의존성과 호환성의 개념
**의존성 (Dependency)**
- 애플리케이션이 실행되기 위해 필요한 **외부 라이브러리**나 **구성 요소**를 의미.
- 예: 스프링 애플리케이션이 동작하려면 `spring-core`, `spring-web`, `jackson-databind`와 같은 라이브러리가 필요.

**호환성 (Compatibility)**
- **애플리케이션 환경**(JDK, 라이브러리 버전 등)과 **사용 라이브러리 간의 적합성**을 의미.
- 예: 특정 버전의 스프링은 특정 JDK 버전과 호환 가능.
  - 스프링 5는 JDK 8 이상에서 동작.
  - 부트 3.0은 JDK 17 이상이 필요.

---

### 2. 일반 웹 프로젝트와 스프링 프로젝트의 차이: 의존성과 호환성
**(1) 일반 웹 프로젝트**
- **의존성 관리**:
  - 라이브러리를 수동으로 다운로드하여 프로젝트에 추가 (`.jar` 파일을 직접 넣음).
  - 의존성 충돌이나 버전 불일치 문제 발생 가능.
  - 모든 경로(classpath)를 개발자가 직접 관리.
- **호환성 문제**:
  - 특정 라이브러리가 다른 라이브러리와 호환되지 않을 수 있음.
  - 문제 발생 시 개발자가 수작업으로 해결해야 함.

**(2) 스프링 프로젝트**
- **의존성 관리**:
  - Maven 또는 Gradle과 같은 **빌드 도구**를 사용하여 의존성을 관리.
  - 프로젝트에 필요한 의존성을 선언하면, 관련된 모든 의존성이 **자동으로** 추가됨 (트랜스티브 의존성 관리).
    ```xml
    <!-- pom.xml 예시 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>5.3.10</version>
    </dependency>
    ```
  - 의존성 충돌 문제는 빌드 도구가 우선순위(예: Maven의 Dependency Tree)를 통해 해결.
- **호환성 관리**:
  - 스프링의 의존성은 호환성이 검증된 라이브러리 집합을 제공 (Spring BOM - Bill of Materials).
  - BOM을 통해 스프링과 호환되는 라이브러리 버전을 자동으로 관리.
    ```xml
    <!-- BOM 선언 예시 -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>3.0.0</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    ```

---

### 3. 의존성과 호환성의 스프링 활용 사례
**(1) 의존성 주입 (Dependency Injection, DI)**
- 스프링 컨테이너는 애플리케이션의 의존성을 자동으로 해결하고 객체 간의 관계를 관리.
- 개발자는 객체 생성이나 의존성 주입을 직접 관리하지 않아도 됨.
- 예:
 ```java
  @Service
  public class UserService {
      private final UserRepository userRepository;

      @Autowired
      public UserService(UserRepository userRepository) {
          this.userRepository = userRepository; // 의존성 주입
      }
  }
``` 


**(2) 호환성 보장 (Spring Boot Starter)**
- 스프링 부트는 다양한 **Starter 패키지**를 제공하여 의존성 충돌과 호환성 문제를 최소화.
- 예: `spring-boot-starter-web`은 다음을 포함:
  - `spring-web`
  - `jackson-databind`
  - `spring-aop`

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>3.0.0</version>
</dependency>
```
qwqw

---

### 4. 스프링 컨테이너와 빌드 도구의 역할

#### (1) 빌드 도구(Maven/Gradle)

- 프로젝트에 필요한 의존성을 선언하면 빌드 도구가 관련 라이브러리를 모두 다운로드.
- 의존성 충돌 해결: 동일한 라이브러리의 여러 버전이 있을 경우, 우선순위를 통해 자동 조정.
- 호환성 관리: BOM 파일을 통해 라이브러리 버전이 스프링 버전과 호환되도록 보장.
#### (2) 스프링 컨테이너
- 의존성 주입(DI)과 객체의 생명 주기 관리.
- 개발자는 비즈니스 로직 구현에만 집중할 수 있음.
- 호환성 문제를 최소화:
- 스프링 컨테이너가 사용하는 라이브러리의 버전은 BOM에 의해 검증됨.
 

### 5. 요약: 의존성과 호환성에서의 스프링 장점
- **의존성**:
  - Maven/Gradle과 스프링 컨테이너가 협력하여 의존성을 자동으로 관리.
  - 개발자는 필요한 기능을 간단히 선언하면 의존성이 자동으로 해결됨.
- **호환성**:
  - 스프링 BOM을 통해 모든 의존성 간의 호환성을 보장.
  - 라이브러리 충돌이나 버전 불일치 문제를 최소화.