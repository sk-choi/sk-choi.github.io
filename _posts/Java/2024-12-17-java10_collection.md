---
title : "[Java] 컬렉션(List, Set, Map)"
date : 2024-12-17 23:20:30 +/-TTTT
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

이 글에서는 자바의 컬렉션에 대해서 다룹니다.

* * *

### 컬렉션이란?

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&amp;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FVNcln%2FbtrH1a7RMGx%2FO1weSEPf18kQvD2OuQpYZ0%2Fimg.png" alt="img" width="597" height="304" class="jop-noMdConv">

이미지 출처 : https://kadosholy.tistory.com/117 \[KADOSHoly:티스토리\]

자바에서 컬렉션(collection)이란 데이터의 집합 혹은 그룹을 의미한다. 많은 수의 데이터를 그 사용 목적에 적합한 자료구조로 묶어 하나로 그룹화한 객체를 말한다.

그리고 컬렉션은 `public interface List<E> extends Collection<E> {   ...   }` 처럼 제네릭 형태로 구현이 되어있다.

**컬렉션의 구조**

<img src="https://blog.kakaocdn.net/dn/CiJc0/btq8wmvLAc0/SazMn8zAYuSFoQsY1N3CH0/img.jpg" alt="img" width="695" height="395" class="jop-noMdConv">

자바의 컬렉션은 Iterable&lt;E&gt;를 최상단 부모 인터페이스로 두고 있다.

Java api doc의 [다음 페이지](https://docs.oracle.com/javase/8/docs/api/)를 보면 Collection 구조에 대해서 더 자세히 알 수 있는데,

> <span style="color: #474747;">The root interface in the</span> *collection hierarchy*<span style="color: #474747;">. A collection represents a group of objects, known as its</span> *elements*<span style="color: #474747;">. Some collections allow duplicate elements and others do not. Some are ordered and others unordered. The JDK does not provide any</span> *direct* <span style="color: #474747;">implementations of this interface: it provides implementations of more specific subinterfaces like</span> Set <span style="color: #474747;">and</span> List<span style="color: #474747;">. This interface is typically used to pass collections around and manipulate them where maximum generality is desired.</span>

<span style="color: #474747;">이 부분에서 public interface Collection&lt;E&gt;가 컬렉션의 계층 구조에서 뿌리(가장 조상인) 인터페이스임을 명시하고 있다. 그리고 이 인터페이스에서 Set과 List를 제공한다고 나와있다. 그렇다면 Map은 어떤 인터페이스에서 제공하는 것일까?</span>

<span style="color: #474747;">거기에 대한 해답은 [여기](https://docs.oracle.com/javase/8/docs/api/)에서 확인할 수 있다. 바로 public interface Map&lt;K,V&gt;이다.</span>

> <span style="color: #474747;"><span style="color: #474747;">The</span> Map <span style="color: #474747;">interface provides three</span> *collection views*<span style="color: #474747;">, which allow a map's contents to be viewed as a set of keys, collection of values, or set of key-value mappings. The</span> *order* <span style="color: #474747;">of a map is defined as the order in which the iterators on the map's collection views return their elements. Some map implementations, like the</span> TreeMap <span style="color: #474747;">class, make specific guarantees as to their order; others, like the</span> HashMap <span style="color: #474747;">class, do not.</span></span>

<span style="color: #474747;">이 부분을 읽어보면 Map 인터페이스는 키의 집합, 값의 컬렉션, 또는 키-값 매핑의 집합으로 볼 수 있도록 하는 세 가지 컬렉션 뷰를 제공하며 TreeMap, HashMap과 같은 클래스가 여기에 속해 있다는 것을 알 수 있다.</span>

* * *

### 제네릭이란?

제네릭(generics)은 한 번의 정의로 여러 종류의 데이터의 타입을 다룰 수 있도록 하는 방법을 말한다.

`public interface List<E> extends Collection<E> {   ...   }`

여기에 나와있는 E를 String, Integer 등 다양한 데이터 타입의 클래스로 변환할 수 있다. 즉 사용자가 사용하고자 하는 데이터 타입을 지정할 수 있는 것이다.

**예시**

`List<String> list = new ArrayList<String>();`

`Set<Integer> set = new Set<Integer>();`

`Map<Integer, String> map = new HashMap<Integer, String>();`

* * *

### 대표적인 컬렉션 인터페이스 : List, Set, Map

인덱스 : List, Set : 인덱스 접근

키 : Map(K,V) : 키 접근

배열은 코딩 테스트에서 사용할 것. (현업에서 배열로 데이터를 담는 시도는 위험하다.)

<span style="color: #202124;">Iterable &lt;E&gt; = Interface Map&lt;K,V&gt;</span>  
<span style="color: #202124;">+Collection &lt;E&gt;</span>  
<span style="color: #202124;">++\[인터페이스\] List&lt;E&gt;, Set&lt;E&gt;, Queue&lt;E&gt;</span>  
<span style="color: #202124;">++\[클래스\] \*\*ArrayList, LinkedList, \*\*HashSet, Stack, Vector</span>

&nbsp;

참고로 이중 리스트(?)에서는 list.get(0).get(0)처럼 사용할 수 있다.

이중 리스트는 사용 비추

| 구분  | 배열 (Array) | 컬렉션 (Collection) |
| --- | --- | --- |
| 데이터 타입 | 같은 타입 | 다양한 타입 |
| 길이 조회 방법 | `length` | `size()` |
| 길이 특성 | 고정적 | 가변적 |
| 요소 접근 방법 | `arr[0]` | `list.get(0)` |

&nbsp;

컬렉션을 사용하기 위해서 java.util.\*을 import 해준다.

### List 선언 및 사용 방법

`List<String> list = new ArrayList<String>();`

**예제 코드**

```java
class Main {
    public static void main (String[] args) throws java.lang.Exception {
        
        List<String> list = new ArrayList<String>();
        
        list.add("hello");
        list.add("nice to");
        list.add("meet you");
        
        for (int i = 1; i <= list.size(); i++){
            System.out.println(list.get(i-1));
        }
    }
}
```

**출력 결과**

> **hello**  
> **nice to**  
> **meet you**

* * *

### Set 선언 및 사용 방법

`Set<Integer> set = new HashSet<Integer>();`

Set은 이름처럼 집합과 같은 특성을 지닌다.

**예제 코드**

```java
class Main {
    public static void main (String[] args) throws java.lang.Exception {
        
        Set<Integer> set = new HashSet<Integer>();
        
        set.add(1);
        set.add(2);
        set.add(3);
        set.add(1);
        
        System.out.println(set.toString());
       
    }
}
```

**출력 결과**

> \[1, 2, 3\]

출력 결과에서 알 수 있듯이 중복을 허용하지 않는다.

* * *

### Map 선언 및 사용 방법

`Map<String, String> map = new HashMap<String, String>();`

Map은 키(key)값과 value로 이루어져 있으며, 키를 통해 해당 갑에 접근할 수 있다.

map.put("key", "value")처럼 put()을 통해서 데이터를 생성하고

map.get("key") 처럼 get()을 통해서 value에 접근한다.

**예제 코드**

```java
class Main {
    public static void main (String[] args) throws java.lang.Exception {
        
        Map<String, String> map = new HashMap<String, String>();
        
        map.put("empno", "7727");
        map.put("ename", "smith");
        map.put("sal", "7000");
        map.put("empno", "3328");
        
        System.out.println(map.toString());
       
    }
}
```

**출력 결과**

> **{ename=smith, empno=3328, sal=7000}**

&nbsp;출력 결과에서 알 수 있듯이 같은 키 값에 대해서 중복으로 입력된 값에 대해 중복을 허용하지 않고, 가장 최근에 입력된 값으로 갱신하게 한다.

* * *

### List에 Map넣기

리스트에 map을 넣을 수도 있다.

```java
class Main {
    public static void main (String[] args) throws java.lang.Exception {
       List<Map<String, String>> qlist = new ArrayList<Map<String, String>>();
        
        Map<String, String> subMap = new HashMap<String, String>();
        subMap.put("empno", "7876");
        subMap.put("ename", "ADAMS");
        subMap.put("sal", "1100");
        qlist.add(subMap);
        
        Map<String, String> subMap2 = new HashMap<String, String>();
        subMap2.put("empno", "7499");
        subMap2.put("ename", "ALLEN");
        subMap2.put("sal", "1600");
        qlist.add(subMap2);
        
        for (int i = 1; i <= qlist.size(); i++) {
            System.out.println(qlist.get(i-1).toString());
        }
    }
}
```

**출력결과**

> **{ename=ADAMS, empno=7876, sal=1100}**  
> **{ename=ALLEN, empno=7499, sal=1600}**

&nbsp;

* * *

### List에 VO 넣기

**VO 코드**

```java
package collection;

public class VO {
    private String name;
    public String getName() {
        return name;
    }
    
    public String getEvery() {
        return this.name + "\t" + this.quatos + "\t" + data; 
    }

    public void setName(String name) {
        this.name = name;
    }


    public String getQuatos() {
        return quatos;
    }

    public void setQuatos(String quatos) {
        this.quatos = quatos;
    }

    public int getData() {
        return data;
    }

    public void setData(int data) {
        this.data = data;
    }

    private String quatos;
    private int data;
    
    VO(String name, String quatos, int data){
        this.name = name;
        this.quatos = quatos;
        this.data = data;
    }
}
```

&nbsp;

**ArrayList&lt;VO&gt; 코드**

```java
package collection;

import java.util.*;

public class ListTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        ArrayList<VO> alist = new ArrayList<VO>();
        
        VO vo1 = new VO("플라톤", "우헤헤", 30);
        VO vo2 = new VO("니체", "크크크", 35);
        VO vo3 = new VO("공자", "히히히", 50);
        
        alist.add(vo1);
        alist.add(vo2);
        alist.add(vo3);
        
        for (int i = 0; i < alist.size(); i++) {
            System.out.println(alist.get(i).getEvery());
        }
    }

}

```
 
**출력 결과**

> **플라톤 우헤헤 30**
> 
> **니체 크크크 35**
> 
> **공자 히히히 50**