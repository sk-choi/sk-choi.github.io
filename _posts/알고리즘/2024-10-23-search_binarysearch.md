---
title : "탐색 알고리즘(이진 탐색) 그리고 bsearch"
date : 2024-10-23 18:36:30 +/-TTTT
categories : 
- Algorithm
tags : 
- [알고리즘] #소문자만 가능
published : true
#permalink : categories/algorithm

#header :
  #teaser : 

toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
---

### Intro

이 글은 탐색 알고리즘 중에서 이진 탐색에 대한 개념과 특징 및 구현 과정을 담은 글입니다.

* * *

### 이진 탐색(Binary Search)의 개념 및 특징

<img src="https://images.velog.io/images/emily0_0/post/789a9b80-9c07-4056-af62-8aa4d4529800/bePceUMnSG-binary_search_gif.gif" alt="img" width="583" height="306" class="jop-noMdConv">

**개념:** 이진 탐색은 정렬된 데이터에서 사용할 수 있는 고속 탐색 알고리즘이다. 탐색 범위를 1/2씩 줄여나가는 방식 때문에 이진 탐색이라는 이름이 붙여졌다.

&nbsp;

**탐색 수행 과정**

이진 탐색의 수행 과정은 다음과 같다.

**1\. 데이터 중앙에 있는 요소를 고른다.**

**2\. 찾는 값이 중앙 요소값보다 큰지 작은지 비교 한다.**

**3\. 찾는 값이 중앙 요소값보다 작으면 중앙 왼편으로, 크다면 중앙 오른편으로 이진 탐색을 수행한다.**

**4\. 찾는 값을 찾을 때까지 1~3 과정을 반복한다.**

&nbsp;

**이진 탐색의 성능**

이진 탐색은 탐색을 시도할 때마다 대상 범위가 1/2로 줄어드는 특징을 가지고 있다. 그리고 이렇게 줄어든 범위가 1이 될 때까지 반복을 수행한다.

이를 수식으로 표현하면, 데이터의 크기가 n, 탐색 반복 횟수를 x라고 한다면 탐색 완료 시점에서 데이터 범위의 크기가 1이 되는 것이다.

즉 수식은 **1 = n \* (1/2)<sup>x</sup>** **\-> 1 = n \* (1/2<sup>x</sup>) -> 2<sup>x</sup> = n -> x = log<sub>2</sub>n** 이 된다. 결과적으로 이진 탐색의 최대 반복 횟수는 **log<sub>2</sub>n**이다.

* * *

**이진 탐색 구현 과정**

```c
#include <stdio.h>

void binarysearch(int arr[], int target, int start, int length){
    
    if (start > length){
        return;
    }
    
    int median = start + (length - start) / 2; // 중앙 값을 그냥 배열 길이의 1/2로 선언하면 다음 재귀 때
                                               //중앙 값보다 큰 범위를 탐색할 경우에 에러가 발생한다.
    if (arr[median] == target){
        printf("%d", arr[median]);
    }
    else if (arr[median] < target){
        binarysearch(arr, target, median+1, length);
    }
    else{
        binarysearch(arr, target, start, median-1);
    }
}

int main(void){
    
    int arr[] = {1,2,3,4,5}; //이미 정렬되었다고 가정. 정렬이 되지 않았다면 정렬 과정 반드시 필요!
    int length = sizeof(arr)/sizeof(arr[0])-1;
    
    binarysearch(arr, 3, 0, length);
    
    //printf("찾는 수 : %d", find);
}
```

**출력 결과**

> 3

코드를 보면 알 수 있듯이 재귀를 통해 범위를 좁혀나가는 과정을 반복한다.

* * *

### C언어 표준 라이브러리의 bsearch()

C언어 표준 라이브러리에 퀵 정렬을 구현한 qsort()가 있는 것처럼, 이진 탐색을 구현한 bsearch()가 존재한다. bsearch() 함수의 원형은 다음과 같다.

```c
void* bsearch(
    const void* key, // 찾고자 하는 값의 데이터 주소
    const void* base, // 데이터 배열의 주소
    size_t num, // 데이터 요소의 개수
    size_t width, // 한 데이터 요소의 크기(byte 단위)
    int (_ _cdecl* compare)(const void*, const void*) // 비교 함수에 대한 포인터
);
```

bsearch()를 사용한 이진 탐색 구현은 아래와 같다.

* * *

### bsearch()를 사용한 이진 탐색 구현 과정

우선 bsearch()를 사용하기 위해선 앞에서 언급한 비교 함수를 우선적으로 선언해야 한다.

**비교 함수인 Compare 선언**

```c
int Compare(const void* _elem1, const void* _elem2){
    
    int* elem1 = (int*)_elem1;
    int* elem2 = (int*)_elem2;
    
    if (*elem1 > *elem2){
        return 1;
    }
    else if (*elem1 < *elem2){
        return -1;
    }
    else{
        return 0;
    }
}
```

비교 함수인 Compare를 선언하는 과정은 앞의 qsort()에서 비교 함수를 선언하는 과정과 동일하다.

이제 bsearch()를 통해 원하는 요소를 찾을 수 있지만, 그 전에 우선 배열을 정렬할 필요가 있다.

빠른 정렬을 위해서 qsort()를 사용하자. 전체 코드는 아래와 같다.

**전체 코드**

```c
#include <stdio.h>
#include <stdlib.h>

int Compare(const void* _elem1, const void* _elem2){
    
    int* elem1 = (int*)_elem1;
    int* elem2 = (int*)_elem2;
    
    if (*elem1 > *elem2){
        return 1;
    }
    else if (*elem1 < *elem2){
        return -1;
    }
    else{
        return 0;
    }
}

int main(void){
    
    int arr[] = {3,1,4,2,5};
    
    int length = sizeof(arr)/sizeof(arr[0]);
    
    qsort((void*)arr, length, sizeof(int), Compare);
    
    int target = 3;
    
    int* find;
    find = bsearch(&target, arr, length, sizeof(int), Compare);
    
    printf("%d", *find);
    
    
    return 0;
}
```

**출력 결과**

> 3

&nbsp;

&nbsp;