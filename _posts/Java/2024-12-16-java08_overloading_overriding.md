---
title : "[Java] 오버 로딩과 오버 라이딩"
date : 2024-12-16 22:40:30 +/-TTTT
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

이 글에서는 오버로딩과 오버라이딩의 특징과 차이점에 대해서 다룹니다.

* * *

### 오버로딩이란?

오버로딩이란 한 클래스 내에 같은 이름의 메서드를 여러 개 정의하는 것을 말한다.

자바에서는 한 클래스 내에 이미 사용하려는 이름과 같은 이름을 가진 메서드가 있더라도 매개변수의 개수 또는 타입이 다르면 같은 이름을 사용해서 메서드를 정의할 수 있다.

**오버 로딩의 성립 조건**

1.  메서드 이름이 같아야 한다.
2.  매개변수의 개수 또는 타입이 달라야 한다.

반환 타입은 오버 로딩에 아무런 영향을 주지 못한다는 것을 명심하자.

**예시 코드**

```java
package overloadingtest;

public class OverloadTest {

    int add(int a, int b) {
        return a + b;
    }
    
    int add(int a, int b, int c) {
        return a + b + c;
    }
    
    long add(long a, long b) {
        return a + b;
    }
    
    double add(double a, double b, double c) {
        return a + b + c;
    }
    
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        OverloadTest ot = new OverloadTest();
        System.out.println(ot.add(1, 2));
        System.out.println(ot.add(1, 2, 3));
        System.out.println(ot.add(2147483648L, 2147483648L));
        System.out.println(ot.add(1.23, 3.45, 4.56));
    }
}
```

**출력결과**

> 3
> 
> 6
> 
> 4294967296
> 
> 9.239999999999998

* * *

### 오버 라이딩이란?

오버라이딩이란 부모클래스에서 정의한 메서드를 자식 클래스에서 재정의해서 사용하는 것을 의미한다.

**오버 라이딩의 성립 조건**

1.  메서드의 이름, 매개변수, 반환 타입이 동일해야 한다.
2.  접근제어자는 부모 클래스의 메서드보다 더 제한적일 수 없다.
3.  부모 메서드에 선언된 예외보다 더 많은 예외를 던질 수 없다.

**이 밖의 특징**

1.  @Override 애너테이션 : 오버라이딩임을 명시적으로 나타내기 위해 @Override를 사용한다.
2.  실제 객체의 타입에 따라 호출되는 메서드가 결정된다.

**예시코드**

```java
package overridingtest;

class Parent {
    
    int data = 0;
    
    public void print() {
        System.out.println("Hello!");
    }
    
    public int add(int a, int b) {
        return a + b;
    }
}

public class OverridingTest extends Parent {

    int data = 1;
    
    //오버 라이딩
    public void print() {
        System.out.println("Nice to meet you!");
    }
    
    public int add(int c, int d) {
        return (c + d) * 2;
    }
    
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        OverridingTest ot = new OverridingTest();
        ot.print();
        System.out.println(ot.add(1, 3));
        
    }
}
```

**출력 결과**

> Nice to meet you!
> 
> 8

* * *

### 정리

| **구분** | **오버로딩(Overloading)** | **오버라이딩(Overriding)** |
| --- | --- | --- |
| **정의** | 같은 클래스 내에서 같은 이름의 메서드를 **다른 매개변수**로 정의하는 것. | 부모 클래스의 메서드를 자식 클래스에서 **재정의**하는 것. |
| **동작 시점** | \*\*컴파일 타임(Compile-time)\*\*에 호출 메서드가 결정됨. | \*\*런타임(Runtime)\*\*에 호출 메서드가 결정됨. |
| **메서드 이름** | 동일해야 함. | 동일해야 함. |
| **매개변수** | 매개변수의 **개수나 타입**이 달라야 함. | 매개변수는 **완전히 동일**해야 함. |
| **반환 타입** | 반환 타입이 달라도 성립 가능. | 반환 타입이 부모 메서드와 **동일**해야 함. |
| **애너테이션** | `@Override` 애너테이션 사용 불가. | `@Override` 애너테이션 사용 가능. |
| **클래스 관계** | **같은 클래스** 내에서 발생. | **부모-자식 클래스 간**에 발생. |
| **주된 목적** | 메서드의 이름을 재사용하여 **유사한 동작을 그룹화**. | 부모 클래스의 동작을 **자식 클래스에 맞게 재정의**. |