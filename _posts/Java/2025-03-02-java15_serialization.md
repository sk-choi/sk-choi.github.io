---
title : "[Java] 동기화와 직렬화"
date : 2025-03-02 00:39:30 +/-TTTT
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

이 글에서는 자바의 **동기화(Synchronization)**와 **직렬화(Serialization)**에 대해 다룬다.

---

### 동기화(Synchronization)란?
- 동기화는 **순차적으로 처리되는 것**을 의미한다.
- 한 번 요청이 들어오면, 해당 요청에 대한 응답이 완료될 때까지 다른 요청에 대한 응답을 하지 않는다.
- **임계영역(Critical Section)**에서 한 번에 하나의 스레드만 실행되도록 보장한다.

### 동기화의 목적
- **데이터의 무결성 보장**:
  - 여러 스레드가 공유 자원을 동시에 수정하면 발생할 수 있는 데이터 충돌을 방지한다.
- **프로그램 안정성 향상**:
  - 멀티스레드 환경에서 예측 가능한 동작을 유지한다.

### synchronized 키워드
- 자바에서 동기화를 구현하기 위해 `synchronized` 키워드를 사용한다.
- **메서드 동기화**와 **블록 동기화** 두 가지 방식으로 사용할 수 있다.

#### 메서드 동기화
- 메서드 전체를 동기화한다.

```java
public class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}
```

#### 블록 동기화
- 특정 코드 블록만 동기화하여 성능을 향상시킨다.

```java
public class Counter {
    private int count = 0;

    public void increment() {
        synchronized (this) {
            count++;
        }
    }

    public int getCount() {
        synchronized (this) {
            return count;
        }
    }
}
```

### 주의사항
- 동기화는 **성능 저하**를 초래할 수 있으므로 필요한 부분에서만 사용해야 한다.

---

### 직렬화(Serialization)란?
- 객체의 상태를 **전송 가능한 형태(바이트 스트림)**로 변환하는 프로세스이다.
- 반대로, 저장된 바이트 스트림을 객체로 복원하는 과정을 **역직렬화(Deserialization)**라고 한다.

### 직렬화의 목적
- **데이터 저장**: 객체를 파일, 데이터베이스 등에 저장할 수 있다.
- **네트워크 전송**: 객체를 네트워크를 통해 전송할 수 있다.

### 직렬화와 역직렬화의 흐름
- 직렬화: `alist ---> 직렬화 ---> 바이트`
- 역직렬화: `alist <--- 역직렬화 <--- 바이트`

### 직렬화 구현 방법
- `Serializable` 인터페이스를 구현하면 해당 클래스의 객체를 직렬화할 수 있다.
- 자바의 직렬화는 `ObjectOutputStream`과 `ObjectInputStream` 클래스를 사용한다.

#### 예제 코드
```java
import java.io.*;

class Person implements Serializable {
    private static final long serialVersionUID = 1L;
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}

public class SerializationExample {
    public static void main(String[] args) {
        Person person = new Person("John", 30);

        // 직렬화
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.ser"))) {
            oos.writeObject(person);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 역직렬화
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.ser"))) {
            Person deserializedPerson = (Person) ois.readObject();
            System.out.println("Deserialized: " + deserializedPerson);
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

### 주요 개념
1. **스트림(Stream)**:
   - 데이터가 전송되는 통로로, 직렬화와 역직렬화에서 사용된다.
2. **transient 키워드**:
   - `transient`로 선언된 필드는 직렬화 대상에서 제외된다.
   ```java
   private transient String password;
   ```
3. **serialVersionUID**:
   - 클래스의 버전을 명시적으로 선언하여 직렬화된 객체와 클래스가 일치하는지 확인한다.
   ```java
   private static final long serialVersionUID = 1L;
   ```

---

## 마샬링(Marshalling)
- 마샬링은 객체를 전송 가능한 형태로 변환하는 작업을 의미한다.
- **직렬화**보다 더 다양한 방식(json, xml 등)을 지원한다.

### 마샬링의 특징
- 직렬화는 바이트 스트림만 처리하지만, 마샬링은 **JSON, XML 등의 형식**으로도 객체를 변환할 수 있다.

---

### 동기화와 직렬화 비교
| **특징**              | **동기화(Synchronization)**          | **직렬화(Serialization)**             |
|-----------------------|-------------------------------------|---------------------------------------|
| 목적                  | 멀티스레드 환경에서 데이터 무결성 보장 | 객체를 저장하거나 전송 가능하게 변환  |
| 주요 키워드            | `synchronized`                      | `Serializable`, `transient`           |
| 적용 대상             | 멀티스레드 코드                      | 객체                                   |

---

### 요약
- **동기화**는 멀티스레드 환경에서 데이터의 무결성을 보장하기 위한 기법이다.
- **직렬화**는 객체를 바이트 스트림으로 변환하여 저장하거나 전송하는 과정이다.
- **마샬링**은 JSON이나 XML 형식으로 객체를 변환하는 작업을 포함한다.
- 각각의 개념은 다른 용도로 사용되며, 프로그램의 안정성과 확장성을 높이는 데 기여한다.
