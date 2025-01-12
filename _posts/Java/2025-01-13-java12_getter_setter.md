---
title : "[Java] 접근 제어자와 getter & setter"
date : 2025-01-13 00:03:30 +/-TTTT
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

이 글에서는 자바의 접근 제어자와 Getter, Setter에 대해 다룹니다.

* * *

### 접근 제어자란?

- 접근 제어자는 클래스, 변수, 메서드, 생성자 등의 접근 범위를 설정하는 키워드이다.
- 적절한 접근 제어자를 사용하면 코드의 보안성과 캡슐화를 강화할 수 있다.
- 자바에는 `public`, `default`, `protected`, `private` 네 가지 접근 제어자가 있다.

* * *

### public

- `public`으로 선언된 요소는 **모든 클래스에서 접근 가능**하다.
- 동일 패키지 여부나 상속 여부와 관계없이 사용할 수 있다.
- 주로 클래스의 인터페이스를 정의하거나 외부에 공개해야 할 메서드나 변수를 선언할 때 사용된다.

```java
public class Example {
    public String message = "Hello, World!";
}
```

### default (패키지 접근 제어자)

- 접근 제어자를 명시하지 않으면 기본적으로 `default` 접근 제어자가 적용된다.
- 같은 패키지 내에서는 접근 가능하지만, 다른 패키지에서는 접근할 수 없다.
- 주로 내부 구현을 같은 패키지 내에서만 사용할 때 적합하다.

```java
class Example {
    String message = "Hello, World!";
}
```

### protected

- `protected`로 선언된 요소는 **같은 패키지**와 **상속받은 클래스에서 접근 가능**하다.
- 외부 클래스에서는 접근할 수 없지만, 상속받은 클래스에서는 제한적으로 사용할 수 있다.
- 주로 상속 관계에서 부모 클래스의 중요한 메서드나 변수를 자식 클래스에 노출할 때 사용된다.

```java
class Parent {
    protected String message = "Hello from Parent";
}

class Child extends Parent {
    public void displayMessage() {
        System.out.println(message); // 가능
    }
}
```

### private

- `private`로 선언된 요소는 **선언된 클래스 내부에서만 접근 가능**하다.
- 외부 클래스, 같은 패키지, 상속받은 클래스에서도 접근할 수 없다.
- 주로 클래스의 내부 데이터를 보호하고 외부에서 직접 수정할 수 없도록 할 때 사용된다.

```java
public class Example {
    private String message = "Hello, World!";
}
```

* * *

### Getter와 Setter

- `private` 접근 제어자를 통해 데이터를 보호하면서, 외부에서 데이터에 접근하고 수정할 수 있는 방법을 제공하기 위해 **Getter**와 **Setter** 메서드를 사용한다.
- **Getter**는 변수 값을 읽는 메서드이고, **Setter**는 변수 값을 수정하는 메서드이다.
- 캡슐화를 강화하고 데이터에 대한 제어를 추가할 수 있다.

```java
public class Example {
    private String message;

    // Getter
    public String getMessage() {
        return message;
    }

    // Setter
    public void setMessage(String message) {
        this.message = message;
    }
}

// 사용 예시
Example example = new Example();
example.setMessage("Hello, Getter and Setter!");
System.out.println(example.getMessage());
```

* * *

### Getter와 Setter의 장점

1.  **캡슐화**:
    - 데이터를 외부에서 직접 접근하지 못하게 하여 클래스 내부의 구현을 보호한다.
2.  **제어 가능**:
    - 값을 설정하거나 읽는 과정을 메서드로 정의하기 때문에 추가적인 로직(유효성 검사 등)을 포함할 수 있다.
3.  **유연성**:
    - 특정 필드의 읽기 전용 또는 쓰기 전용으로 설계할 수 있다.

```java
public class Example {
    private int age;

    // 읽기 전용 Getter
    public int getAge() {
        return age;
    }

    // 쓰기 전용 Setter
    public void setAge(int age) {
        if (age > 0) { // 유효성 검사
            this.age = age;
        }
    }
}
```

### 요약

- **`public`**: 모든 클래스에서 접근 가능.
- **`default`**: 같은 패키지에서만 접근 가능.
- **`protected`**: 같은 패키지 및 상속받은 클래스에서 접근 가능.
- **`private`**: 선언된 클래스 내부에서만 접근 가능하며, 데이터 보호를 위해 주로 Getter와 Setter를 사용한다.
- Getter와 Setter는 캡슐화를 강화하고 데이터 제어를 가능하게 한다.