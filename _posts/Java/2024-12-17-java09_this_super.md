---
title : "[Java] this와 this(), super와 super()의 차이점"
date : 2024-12-17 00:09:30 +/-TTTT
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

이 글에서는 자바의 this와 this(), super와 super()의 차이점에 대해서 다룹니다.

* * *

### this와 this()

**this :** 객체 자신의 변수나 메서드를 참조하는 키워드

주로 인스턴스 변수와 로컬 변수 이름이 충돌할 때 이를 구분하기 위해서 사용.

**this() :** 현재 클래스의 다른 생성자를 호출할 때 사용.

**주의할 점 :**  static 메서드에서는 사용할 수 없다. 그리고 생성자의 첫 번째 줄에서만 사용 가능하다.

기본 생성자가 없고, 사용자 정의 생성자도 없는 경우엔 컴파일러에서 `this()`를 자동으로 삽입한다.

* * *

### super와 super()

**super :** 부모 클래스의 변수 또는 메서드를 참조하는 키워드

부모 클래스의 멤버를 명시적으로 호출할 때 사용

**super() :** 부모 클래스의 생성자를 호출할 때 사용. 생성자의 첫 번째 줄에서만 사용 가능하다.

this()와 마찬가지로 명시하지 않을 경우에 컴파일러가 자동으로 부모 클래스의 기본 생성자를 호출한다.

**주의할 점 :** static 메서드에서는 사용할 수 없다. 그리고 생성자의 첫 번째 줄에서만 사용 가능하다.

부모 클래스의 생성자를 명시적으로 호출하지 않으면, 컴파일러가 기본 생성자인 super()를 자동으로 삽입한다.

* * *

### **테스트 코드**

```java
class Animal {
    int num;

    // 기본 생성자
    Animal() {
        System.out.println("Animal default constructor");
    }

    // 매개변수가 있는 생성자
    Animal(int num) {
        this.num = num;
        System.out.println("Animal parameterized constructor: num = " + num);
    }

    public void move() {
        System.out.println("기본 생성자 : I am moving");
    }
}

class Main extends Animal {
    int num_;

    // 기본 생성자
    Main() {
        this(0); // 생성자 체이닝
        System.out.println("Default constructor called");
    }

    // 매개변수가 있는 생성자
    Main(int a) {
        super(a); // 부모 클래스의 매개변수 생성자 호출
        this.num_ = a; // 현재 객체의 인스턴스 변수 초기화
        System.out.println("Main parameterized constructor: num_ = " + a);
    }

    @Override
    public void move() {
        super.move(); // 부모 클래스의 move() 호출
        System.out.println("오버라이딩 : I am moving fast");
    }

    public void move(int a) {
        System.out.println("오버라이딩 + 오버로딩 : I am moving fast * " + a);
    }

    public void printNum() {
        System.out.println("num_: " + this.num_); // this를 사용해 인스턴스 변수 참조
    }

    public static void main(String[] args) {
        // 기본 생성자 호출
        Main m0 = new Main();
        // 매개변수 생성자 호출
        Main m1 = new Main(5);

        // 기본 생성자로 생성된 객체의 num 출력
        System.out.println("m0.num: " + m0.num); // 부모 클래스의 변수
        // 사용자 정의 생성자의 num_ 출력
        System.out.println("m1.num_: " + m1.num_); // 자식 클래스의 변수

        // 메서드 호출
        m0.move(); // 오버라이딩된 메서드 호출
        m1.move(5); // 오버로딩된 메서드 호출

        // num_ 출력
        m1.printNum();
    }
}

```

**출력결과**

> **Animal parameterized constructor: num = 0**  
> **Main parameterized constructor: num_ = 0**  
> **Default constructor called**  
> **Animal parameterized constructor: num = 5**  
> **Main parameterized constructor: num_ = 5**  
> **m0.num: 0**  
> **m1.num_: 5**  
> **기본 생성자 : I am moving**  
> **오버라이딩 : I am moving fast**  
> **오버라이딩 + 오버로딩 : I am moving fast \* 5**  
> **num_: 5**

* * *

### 비교 정리

| **구분** | **`this`** | **`super`** |
| --- | --- | --- |
| **참조 대상** | 현재 객체의 참조 | 부모 클래스의 참조 |
| **사용 목적** | 인스턴스 변수와 로컬 변수 구분, 생성자 체이닝 | 부모 클래스의 메서드나 변수 접근 |
| **생성자 호출** | 현재 클래스의 다른 생성자 호출 (`this()`) | 부모 클래스의 생성자 호출 (`super()`) |