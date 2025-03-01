---
title : "[Java] Scanner 그리고 System.out 구조 파헤치기"
date : 2025-03-02 00:40:30 +/-TTTT
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

이 글에서는 Scanner와 System.out의 구조에 대해서 다룹니다.

* * *

### Scanner에 대해서

자바에서 입력을 받기 위해서는 Scanner 클래스를 인스턴스화해서 호출해야 한다.

```java
import java.util.Scanner; // Scanner 클래스 호출

Scanner sc = new Scanner(System.in);
```

근데 `Scanner()`안에 들어가는 System은 무엇일까?

이클립스 환경에서 파고 들어가면 System의 기능과 설명에 대해서 알 수 있다. (또는 java api doc을 살펴보자!)

> <span style="color: #474747;">The</span> `System` <span style="color: #474747;">class contains several useful class fields and methods. It cannot be instantiated.</span>
> 
> Among the facilities provided by the `System` class are standard input, standard output, and error output streams; access to externally defined properties and environment variables; a means of loading files and libraries; and a utility method for quickly copying a portion of an array.

`System` 클래스는 여러 유용한 클래스 필드와 메서드를 포함하고 있으며, 인스턴스화할 수 없다.

`System` 클래스가 제공하는 기능에는 표준 입력, 표준 출력 및 에러 출력 스트림, 외부에서 정의된 속성과 환경 변수에 대한 접근, 파일 및 라이브러리를 로드하는 수단, 그리고 배열의 일부를 빠르게 복사하기 위한 유틸리티 메서드가 포함된다.

* * *

### System.in에 대해서

그럼 `System.in`에서 in은 무엇일까?

> The "standard" input stream. This stream is already open and ready to supply input data. Typically this stream corresponds to keyboard input or another input source specified by the host environment or user.

&nbsp;이 문구에서 알 수 있듯이 in은 '표준' 입력 스트림을 의미한다. 즉 자바의 표준적인 '바이트'를 입력받는 형식이라 말할 수 있다.

그리고 해당 in을 통해 키보드 또는 다른 입력 도구에 응답한다.

System.in의 가장 근원의 인터페이스는 AutoCloseable (public interface AutoCloseable)라는 것이다.

- `AutoCloseable` 객체는 파일이나 소켓 핸들 등과 같은 **자원을 보유할 수 있는 객체**를 의미한다.
- `AutoCloseable` 객체의 `close()` 메서드는 해당 객체가 **`try-with-resources`** 블록에서 사용될 때 **블록이 종료되면 자동으로 호출된다.**
- 이 구조는 자원을 신속히 해제할 수 있도록 보장하여, 자원 고갈에 따른 예외나 오류가 발생하는 것을 방지한다.

* * *

### System.out에 대해서

그럼 System.out은 어떤 구조로 이루어져 있을까?

> <span style="color: #474747;">The "standard" output stream. This stream is already open and ready to accept output data. Typically this stream corresponds to display output or another output destination specified by the host environment or user.</span>
> 
> For simple stand-alone Java applications, a typical way to write a line of output data is:
> 
> ```
> System.out.println(data)
> ```

System.out은 표준 출력 스트림이다. 이 스트림은 출력 데이터에 대해 이미 열려있고 수용할 준비가 되어있다. 일반적으로 이 스트림은 출력 또는 사용자에 의해 구체화된(지정된) 출력 목적지를 보여주는 것에 응답한다.

System.out의 가장 근원의 인터페이스는 `AutoCloseable`과 `Flushable`이다.

`Flushable`은 데이터를 전송하는 대상으로, 해당 데이터를 **버퍼에서 비워내는(내보내는)** 기능을 가진 인터페이스다.  
`flush` 메서드는 버퍼에 저장된 출력 데이터를 기본 스트림(underlying stream)으로 전송하기 위해 호출된다.

* * *

**참고자료**

https://st-lab.tistory.com/41

&nbsp;