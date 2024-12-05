---
title : "[Java] String 리터럴 선언과 new를 통한 선언의 차이점"
date : 2024-12-05 21:11:30 +/-TTTT
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

이 글에서는 String변수의 리터럴 선언과 new를 통한 선언의 차이점에 대해 다룬 글입니다.

* * *

### String에 대해서

앞선 글에서 String 자료형은 레퍼런스 타입의 자료형이라는 것을 말한 적이 있다.

레퍼런스라는 이름에서 알 수 있듯이 String 자료형은 메모리 주소와 밀접한 관련을 가진다.

* * *

### String a = "abc"와 String a =  new String("abc")의 차이점

자바에서 String 변수에 값을 할당하기 위한 방법으로는 두 가지가 존재한다.

첫 번째는 `String a = "abc";`와 같이 선언 및 할당을 하는 방식이고,

두 번째는 `String a =  new String("abc");`와 같이 new를 통해 선언 및 할당을 하는 방식이다.

더 자세한 차이를 비교하기 위해서 다음의 코드를 살펴보자.

조건문을 통해 값이 같다면 '같다'를, 다르다면 '다르다'를 출력한다.

**String 선언 방식 결과를 비교하기 위한 코드**

```java
class Main {

    public static void main (String[] args) {
        String str1 = new String("abc"); // 힙 메모리에 값 복제
 		String str2 = "abc"; // constant pool 메모리에 상수 영역을 잡아놓고 여기에 값을 할당. 같은 값이면 같은 주소 할당
 		String str3 = "def";
 		String str4 = "def";
 		
 		// str1과 str2의 주소 비교
 		if (str1 == str2) {
 			System.out.println("같다");
 		}
 		else {
 			System.out.println("다르다");
 		}
 		
 		// str3과 str4의 주소 비교
 		if (str3 == str4) {
 			System.out.println("같다");
 		}
 		else {
 			System.out.println("다르다");
 		}
```

**결과 출력**

> **다르다**  
> **같다**

* * *

### 결과가 다른 이유

str1과 str2는 똑같이 "abc"가 할당되어 있지만, 조건문을 통해 비교한 결과 다르다가 출력되었고,

str3과 str4는 똑같이 "def"가 할당되어 있지만, 같다가 출력되었다.

이러한 결과 차이는 리터럴 선언과 new를 통한 선언의 주소 할당 방식이 다르다는 점에서 발생한다.

<img src="https://velog.velcdn.com/images/carrotboy/post/c0a0e543-e3dc-4261-8f78-89157f692095/image.JPG" alt="img" width="595" height="355" class="jop-noMdConv">

위 그림을 보면 같은 10이라는 같은 값을 가진 변수는 constant pool에서 주소를 공유하고 있지만,

다른 나머지 값들은 heap 영역에서 서로 다른 주소를 할당 받고 있다.

위의 constant pool은 정확히 말한다면 **string constant pool**에 해당한다.(클래스 파일의 constant pool과는 다르다.)

string constant pool은 string pool이라고도 하는데 자바에서 문자열을 저장하는 독립된 영역이며,

문자열 생성시 이곳에 위치한 문자열과 같은 값을 가진 문자열을 생성한다면 이 곳의 주소를 공유하게 한다.

그리고 이러한 방식을 통해 문자열 사용 시 메모리를 절약하게 한다.

결론적으로 리터럴 선언은 string constant pool을 이용하기에 할당한 문자열이 같은 경우 같은 값이 저장된 주소를 할당하지만,

new를 사용한 선언은 선언할 때마다 새롭게 주소가 추가적으로 할당된다.

그렇기에 같은 문자열이 할당 되어도 조건문을 통한 비교 결과가 다른 것이다.

* * *

### 값 자체를 비교하기 위해서는 equals()

이렇게 다른 주소 할당 방식 때문에 값 자체를 비교하기 불가능하다고 생각할 수 있다.

이를 위해서 사용할 수 있는 방식이 바로 `equals()`를 사용하는 것이다.

**사용 예시**

```java
str1.equals(str2)
```

**equals()를 사용한 값 자체를 비교하기 위한 코드**

```java
class Main {

    public static void main (String[] args) {
        String str1 = new String("abc"); // 힙 메모리에 값 복제
 		String str2 = "abc"; // constant pool 메모리에 상수 영역을 잡아놓고 여기에 값을 할당. 같은 값이면 같은 주소 할당
 		
 		String str3 = "def";
 		String str4 = "def";
 		
 		// str1과 str2의 값 비교
 		if (str1.equals(str2)) {
 			System.out.println("같다");
 		}
 		else {
 			System.out.println("다르다");
 		}
 		
 		// str3과 str4의 값 비교
 		if (str3.equals(str4)) {
 			System.out.println("같다");
 		}
 		else {
 			System.out.println("다르다");
 		}
```

**결과 출력**

> **같다**  
> **같다**