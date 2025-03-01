---
title : "[Java] 예외처리(try~catch)"
date : 2025-03-02 00:38:30 +/-TTTT
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

이 글에서는 자바의 **예외 처리(Exception Handling)**에 대해 다룹니다.

***

### 예외란?
- 예외는 **프로그램 실행 중에 발생할 수 있는 예외적인 상황**을 처리하기 위한 기법이다.
- 프로그램이 예외 상황을 잘 처리하여 **비정상 종료를 방지하고 정상적인 실행 상태를 유지**하도록 돕는다.

### 프로그램 오류의 종류
1. **컴파일 에러**:
   - 컴파일 시점에 발생하는 오류이다.
   - 예: 문법 오류, 타입 미스매치 등.
2. **런타임 에러**:
   - 프로그램 실행 중에 발생하는 오류이다.
   - 예: `NullPointerException`, `ArrayIndexOutOfBoundsException` 등.

> **에러와 예외의 차이**:
> - **에러**: 시스템 레벨에서 발생하며, 복구가 어려운 상황.
> - **예외**: 복구 가능한 상황으로, 적절히 처리해야 한다.

### 예외 처리의 목적
- 프로그램을 정상적으로 끝까지 실행시키는 것이 주된 목적이다.
- 예외 처리를 통해 **프로그램의 비정상 종료를 막고**, 정상적인 상태를 유지한다.

---

### try-catch-finally 구조
- 자바에서는 `try-catch-finally` 블록을 사용하여 예외를 처리한다.
- 기본 구조는 다음과 같다:

```java
try {
    // 문제 발생 가능성 있는 코드
} catch(ExceptionType1 e) {
    // ExceptionType1 예외 처리
} catch(ExceptionType2 e) {
    // ExceptionType2 예외 처리
} finally {
    // 무조건 실행되는 블록 (선택 사항)
}
```

### 각 키워드의 역할
1. **try**:
   - 예외가 발생할 가능성이 있는 코드를 포함한다.
2. **catch**:
   - 발생한 예외를 처리한다.
   - **수직적 구조**를 가지며, 구체적인 예외를 먼저 처리한 후 범용적인 예외를 처리한다.
3. **finally**:
   - 예외 발생 여부와 관계없이 **항상 실행**된다.
   - 주로 리소스 정리 코드에 사용된다.

### 예제 코드
```java
public class Main {
    public static void main(String[] args) {
        try {
            int result = 10 / 0; // 예외 발생
        } catch(ArithmeticException e) {
            System.out.println("ArithmeticException 발생: " + e.getMessage());
        } finally {
            System.out.println("프로그램 종료 전 실행 (finally 블록)");
        }
    }
}
```

---

### 예외를 세분화하여 처리하기
- 다양한 예외를 세분화하여 처리함으로써 **효율적인 예외 처리**가 가능하다.
- 각 `catch` 블록은 특정 예외만 처리하며, **처리 후 블록을 빠져나간다**.

```java
try {
    int[] numbers = {1, 2};
    System.out.println(numbers[3]);
} catch(ArrayIndexOutOfBoundsException e) {
    System.out.println("배열 인덱스 초과: " + e.getMessage());
} catch(Exception e) {
    System.out.println("일반 예외 처리: " + e.getMessage());
}
```

---

### 예외 만들기
- 자바에서는 직접 예외를 발생시키거나(`throw`) 호출부로 예외를 넘길 수 있다(`throws`).

### throw 키워드
- **개발자가 강제로 예외를 발생**시킨다.
- 판단에 따라 필요한 상황에서 사용한다.

```java
throw new Exception("강제로 예외 발생");
```

### throws 키워드
- **호출부로 예외를 넘기는 역할**을 한다.
- 예외 처리를 호출한 메서드에게 강요한다.

```java
public void divide(int a, int b) throws ArithmeticException {
    if (b == 0) {
        throw new ArithmeticException("0으로 나눌 수 없습니다.");
    }
    System.out.println("결과: " + (a / b));
}
```

### throw와 throws의 차이

| **구분**        | **throw**                                | **throws**                              |
| --- | --- | --- |
| 역할             | 강제로 예외를 발생시킴                  | 호출한 메서드로 예외를 넘김             |
| 위치             | 메서드 내부에서 사용                     | 메서드 선언부에서 사용                  |
| 사용 목적        | 특정 상황에서 즉시 예외를 발생시킴       | 메서드 호출부에서 예외 처리를 강요함    |
| 예제             | `throw new Exception("예외 메시지");`  | `public void method() throws Exception` |

---

### 요약
- **예외 처리**는 프로그램의 비정상 종료를 방지하고 정상 상태를 유지하기 위해 중요하다.
- `try-catch-finally`를 사용하여 예외를 처리하며, 예외를 **세분화**하여 효율적으로 관리한다.
- `throw`와 `throws`를 사용하여 예외를 발생시키거나 호출부로 넘길 수 있다.
- 적절한 예외 처리는 프로그램의 안정성과 유지보수성을 높인다.
