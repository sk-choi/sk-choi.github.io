---
title : "[Java] 자바의 캐스팅(casting)"
date : 2024-12-05 21:09:30 +/-TTTT
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

이 글에서는 자바(Java)의 캐스팅(casting, 형변환)에 대해서 다룹니다.

* * *

### 캐스팅이란?

캐스팅이란 형 변환, 즉 타입 변환을 의미한다.

캐스팅을 이해하기 위해서는 변수의 크기가 타입(자료형)마다 다르다는 것을 알아야 한다.

자료형 각각에 할당되는 크기가 다르기 때문에, 자료형을 변환하기 위해서는 나름의 규칙이 적용될 수밖에 없다.

그렇기에 캐스팅의 경우를 크기가 큰 자료형에서 작은 자료형으로 변환하는 경우와

크기가 작은 자료형에서 큰 자료형으로 변환하는 경우로 나누어 생각해봐야 할 것이다.

* * *

### 캐스팅의 종류

**업 캐스팅(up-casting) :** 작은 크기의 타입에서 큰 크기의 타입으로 형 변환을 하는 것. 타입 명시를 생략해도 된다.  
**다운 캐스팅(down-casting) :** 큰  크기의 타입에서 작은 크기의 타입으로 형 변환을 하는 것. 무조건 작은 것 타입 명시가 필요!

자바에서 캐스팅을 할 때 주의할 점은 boolean을 제외한 7개의 기본형만 서로 형변환이 가능하다는 것이다.

```java
//<업캐스팅 예시>
//char-->int
int num = c;
//또는 
int num = (int)c;

//<다운캐스팅 예시>
//int --> char
int num = 10;
char c = 'a';
c = (char)num;
```

**이해하기 힘들다면?**

큰집에서 작은 집 갈려면 힘들지만(그래서 명시적 캐스팅 필요) 작은 집에서 큰집 가는 건 쉽다(그래서 생략 가능)이라고 생각하자.

캐스팅 테스트 코드

```java
class Main {
    public static void main (String[] args) {
        int a = 65;
        char A = (char)a;
        System.out.println(A); // A출력
        
        float fl = 0.165f;
        int ifl = (int)fl;
        System.out.println(ifl); // 소수점 버려져서 0 출력
        
        short n1 = 1;
        short n2 = 2;
        //short n3 = n1 + n2; <- 에러 발생
        Short n3 = (short)(n1 + n2);
        System.out.println(n3); // 3출력
        
        char B = 'B';
        int iB = B;
        System.out.println(iB); // 66출력
        
        int integer = 0;
        float fl2 = integer;
        System.out.println(fl2); // 0.0출력
    }
}
```

**출력 결과**

> **A**  
> **0**  
> **3**  
> **66**  
> **0.0**

* * *

### String을 다른 자료형으로 캐스팅하는 방법(Wrapper Class)

그렇다면 String 자료형으로 캐스팅을 하려면 어떻게 해야 할까?

앞에서 언급했듯이 캐스팅은 boolean을 제외한 7개의 기본형에서만 가능하다.

레퍼런스 자료형인 String을 캐스팅하려면 \*\*'래퍼 클래스(Wrapper Class)'\*\*라는 것을 사용한다.

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbvzp79%2FbtqEbacB01v%2FQQjO7cSc9tTvKJkyzFsK90%2Fimg.png)

래퍼 클래스는 각각의 타입에 해당하는 데이터를 인수로 전달 받아, 해당 값을 가지는 객체로 만들어준다.

위 그림을 보면 알 수 있듯이 모든 래퍼 클래스의 부모는 Object이고, 숫자를 다루는 래퍼 클래스의 부모 클래스는 Number인 것을 알 수 있다.

**래퍼 클래스 테스트 코드**

```java
class Main {
    public static void main (String[] args) {
    	String ss = "1";
        char ivv = 'A';
        // System.out.println((int)ss); <-불가!!
        System.out.println(Integer.parseInt(ss) + 3);
        System.out.println(Byte.parseByte(ss));
        System.out.println(Short.parseShort(ss));
        System.out.println(Character.toChars(ivv));
        System.out.println(Float.parseFloat(ss));
        System.out.println(Boolean.parseBoolean(ss));
        System.out.println(Double.parseDouble(ss));
    }
}
```

**출력 결과**

> **4**  
> **1**  
> **1**  
> **A**  
> **1.0**  
> **false**  
> **1.0**

여기서 `Boolean.parseBoolean(ss)`는 `false`가 출력되는 걸 알 수 있는데, `Boolean.parseBoolean()`은 안에 들어가는 값이

문자열 "true" 여야만 `true`를 반환한다. (대소문자 구분 상관 없음)

**Boolean.parseBoolean()로 true 출력**

```java
class Main {
    public static void main (String[] args) {
        String true_ = "true";
        System.out.println(Boolean.parseBoolean(true_));
    }
}
```

**출력 결과**

> **true**
