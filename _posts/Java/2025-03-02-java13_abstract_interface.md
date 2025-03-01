---
title : "[Java] 추상 클래스 그리고 인터페이스"
date : 2025-03-02 00:37:30 +/-TTTT
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

이 글에서는 자바의 **추상 클래스**와 **인터페이스**에 대해 다룹니다.

* * *

### 추상 클래스

- 추상 클래스는 `abstract` 키워드로 선언되며, 객체를 직접 생성할 수 없다.
- 하나 이상의 **추상 메서드**(구현되지 않은 메서드)를 포함할 수 있다.
- 일반 메서드(구현된 메서드)와 필드도 포함할 수 있다.
- 주로 **공통 기능**을 구현하면서, 하위 클래스가 세부 구현을 제공하도록 강제할 때 사용된다.

### 특징

1.  객체를 직접 생성할 수 없으며, 상속을 통해 사용해야 한다.
2.  하나 이상의 추상 메서드를 포함할 수 있다.
3.  구현된 메서드와 필드를 포함할 수 있다.

### 예제 코드

```java
// 추상 클래스 정의
abstract class Animal {
    // 일반 메서드
    public void eat() {
        System.out.println("This animal eats food.");
    }

    // 추상 메서드
    public abstract void makeSound();
}

// 추상 클래스를 상속받는 구체 클래스
class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myDog = new Dog();
        myDog.eat();
        myDog.makeSound();
    }
}
```

### 사용 목적

- 공통 로직을 구현하여 코드 중복을 줄이고, 하위 클래스에서 필요한 메서드만 구현하도록 강제한다.

* * *

### 인터페이스

- 인터페이스는 클래스가 따라야 하는 **계약**을 정의한다.
- `interface` 키워드로 선언되며, 모든 메서드는 기본적으로 **추상 메서드**이다.
- 다중 상속을 지원하여, 클래스가 여러 인터페이스를 구현할 수 있다.
- 자바 8 이후부터 **디폴트 메서드**와 **정적 메서드**를 가질 수 있다.

### 특징

1.  모든 메서드는 기본적으로 `public`이고 `abstract`이다.
2.  자바 8부터 디폴트 메서드와 정적 메서드가 추가되었다.
3.  모든 필드는 기본적으로 `public static final`이다.

### 예제 코드

```java
// 인터페이스 정의
interface Animal {
    void makeSound(); // 추상 메서드
}

interface Pet {
    void play(); // 추상 메서드
}

// 여러 인터페이스 구현
class Dog implements Animal, Pet {
    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }

    @Override
    public void play() {
        System.out.println("The dog is playing.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog myDog = new Dog();
        myDog.makeSound();
        myDog.play();
    }
}
```

### 사용 목적

- 클래스가 특정 행동을 구현하도록 강제하고, 다중 상속의 이점을 제공한다.

* * *

### 추상 클래스와 인터페이스 비교

| **특징** | **추상 클래스** | **인터페이스** |
| --- | --- | --- |
| 키워드 | `abstract` | `interface` |
| 다중 상속 지원 | 불가능 | 가능  |
| 메서드 구현 여부 | 구현된 메서드와 추상 메서드 모두 포함 | 기본적으로 추상 메서드만 포함 (디폴트 메서드 제외) |
| 필드  | 일반 필드 포함 가능 | `public static final` 필드만 포함 |
| 사용 목적 | 공통 로직 제공 + 부분적 구현 강제 | 행동 규약 제공 및 다중 상속 지원 |

* * *

### 요약

- **추상 클래스**는 공통 기능을 제공하며, 일부 메서드를 구현할 수 있다.
- **인터페이스**는 클래스가 구현해야 할 계약을 정의하며, 다중 상속을 지원한다.
- 둘 모두 객체지향 프로그래밍의 핵심 요소로, 설계의 유연성과 확장성을 높여준다.