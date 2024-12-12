---
title : "[Java] 상속과 포함"
date : 2024-12-12 19:27:30 +/-TTTT
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


### <span style="color: #202124;">Intro</span>

<span style="color: #202124;">이 글은 자바의 상속과 포함 관계에 대해서 다룹니다.</span>

* * *

### <span style="color: #202124;">상속이란?</span>

<span style="color: #202124;">상속이란 기존의 클래스를 재사용해서 새로운 클래스를 작성하는 것.</span>  
<br/><span style="color: #202124;">즉, 두 클래스를 조상과 자손으로 관계를 맺어주는 것이라 볼 수 있다.(is-a 관계)</span>

<span style="color: #202124;">상속 관계를 만들기 위해서는 `extends`를 사용한다.</span>

**특징**

- <span style="color: #202124;">자손은 조상의 모든 멤버를 상속 받는다.(생성자, 초기화 블록 제외)</span>
    
- <span style="color: #202124;">자손의 멤버 개수는 조상보다 적을 수 없다.(같거나 많다.)</span>
    
- <span style="color: #202124;"><span style="color: #202124;">공통 부분은 조상에서 관리하고, 개별 부분은 자손에서 관리한다.</span>  
    </span>
    
- <span style="color: #202124;"><span style="color: #202124;">부모 클래스에서의 변경 및 수정은 자식 클래스에 영향을 주지만,</span></span> <span style="color: #202124;"><span style="color: #202124;">자식 클래스에서의 변경 및 수정은 부모 클래스에 영향을 주지 않는다.</span></span>
    
- <span style="color: #202124;"><span style="color: #202124;"><span style="color: #202124;">자바는 클래스의 관계에서 단일 상속만을 허용한다.</span></span></span>
    

&nbsp;

**<span style="color: #202124;"><span style="color: #202124;"><span style="color: #202124;">테스트 코드</span></span></span>**

```java
package javainput;

class Parents {
    
    int data = 10;
    String str = "Hello";
    char a = 'A';
}


public class Child extends Parents{

    int data = 11;
    String str = "Nice to meet you!";
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        Child c = new Child();
        System.out.println(c.data);
        System.out.println(c.str);
        System.out.println(c.a);
    }

}

```

**출력 결과**

> **11**  
> **Nice to meet you!**  
> **A**

출력 결과에서 볼 수 있듯이 자식 클래스에는 `char a`가 선언되어 있지 않지만, 부모 클래스에서 선언하고 할당한 값이 그대로 출력된다는 것을 확인할 수 있다.

* * *

### 포함이란?

포함은 상속과 다르게 한 클래스가 다른 클래스의 객체를 자신의 멤버 변수로 포함하는 관계를 의미한다.(has-a 관계)

**특징**

- 포함 관계는 기존 클래스의 기능을 재사용하면서도, 더 높은 유연성을 제공
- 객체 간 의존성을 느슨하게 유지
- 포함 관계에서는 포함된 객체의 기능을 사용하는 방식으로 동작을 구현

&nbsp;

**테스트 코드**

```java
class Toy {
    
    int data = 10;
    String str = "Hello";
    
    public void play() {
        System.out.println("hahaha!");
    }
}


public class Child {

    int data = 11;
    String str = "Nice to meet you!";
    
    Toy toy;
    
    Child(){
        this.toy = new Toy();
    }
    
    public void enjoy() {
        toy.play();
        System.out.println("I'm Happy!");
    }
    
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        Child child = new Child();
        child.enjoy();
    }

}

```

**출력결과**

> **hahaha!**
> 
> **I'm Happy!**

* * *

###  정리

| **특징** | **상속(Inheritance)** | **포함(Composition)** |
| --- | --- | --- |
| **관계** | is-a (A는 B이다) | has-a (A는 B를 가지고 있다) |
| **재사용성** | 부모 클래스의 속성과 메서드를 상속받아 재사용 | 다른 클래스의 객체를 포함하여 재사용 |
| **유연성** | 부모-자식 관계가 고정적이며 덜 유연함 | 느슨한 결합으로 더 유연한 구조 제공 |
| **다중 관계** | 단일 상속만 지원 | 다중 포함 가능 |
| **구현 방식** | `extends` 키워드로 클래스 확장 | 클래스 내에 다른 클래스 객체를 멤버로 포함 |
| **결합도** | 높은 결합도 | 낮은 결합도 |
| **예제 관계** | `Student extends Person` | `Car has Engine` |