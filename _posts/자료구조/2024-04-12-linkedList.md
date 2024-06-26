---
title : "링크드 리스트(Linked List)"
date : 2024-04-12 23:39:30 +/-TTTT
categories : 
- Data_Structure
tags : 
- [자료구조] #소문자만 가능
published : true
#permalink : categories/data_structure

#header :
  #teaser : 

toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
---

### Intro

이 글은 링크드 리스트 자료구조에 대한 설명을 적은 글입니다.

* * *

### 링크드 리스트(Linked List)란?

노드를 연결해서 만든 리스트로, 여기서 노드는 리스트의 가장 기본적인 단위라고 말할 수 있다.

<img src="https://velog.velcdn.com/images%2Ftataki26%2Fpost%2F0616cb34-45e1-4a19-a053-abdae7c721bd%2Fsbzf3hz07azamnxapyp1.png" alt="img" width="415" height="339">   

위 그림처럼 자료가 담겨 있는 data와 다음 노드를 연결하는 link로 이루어져 있다.

이러한 구조를 가진 노드를 이으면 링크드 리스트가 구현되는 것이다.

링크드 리스트 구조에서 가장 앞의 노드는 head라고 하며, 가장 뒤의 노드는 tail이라고 말한다.

* * *

### 구현을 위한 세부 사항

c언어에서는 이러한 링크드 리스트를 구현하기 위해서 구조체라는 자료형을 사용한다.

그리고 링크드 리스트를 구현하는 데 필요한 함수를 작성할 필요가 있는데 요구되는 기능은 아래와 같다.

1.  노드의 생성(create)과 소멸(DestroyNode)
2.  노드 추가(Append)
3.  노드 탐색
4.  노드 삭제
5.  노드 삽입

* * *

### 구현 예시

구현 예시는 아래의 링크를 참고 바란다.

[링크드 리스트 구현 첫 번째](https://sk-choi.github.io/data_structure/linkedList-gpt/)   


[링크드 리스트 완전 구현(C언어)](https://sk-choi.github.io/data_structure/linkedList_compt/)

* * *

### 링크드 리스트의 특징(배열과의 비교)

**장점:**

01\. 새로운 노드의 추가, 삽입, 삭제가 쉽고 빠르다. 배열은 새로운 요소 삽입과 제거가 어렵다.

02\. 현재 노드의 다음 노드를 얻어오는 연산에 대해서 비용이 발생하지 않는다.(장점도 단점도 아닌 애매한 점)

**단점:**

01\. 다음 노드를 가리키는 포인터로 인해서 각 노드마다 추가적인 메모리가 필요하다.

02\. 특정 위치에 있는 노드에 접근하기 위한 비용이 크고, 접근하는 시간이 오래 걸린다.

(n번째 노드까지 접근하기 위해서 n회의 연산이 필요, 반면에 배열은 상수 시간(첨자 \* 노드크기)에 노드를 얻기가 가능하다.)

&nbsp;


