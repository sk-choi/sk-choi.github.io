---
title : "정렬 알고리즘(버블, 선택, 삽입)"
date : 2024-10-17 14:47:30 +/-TTTT
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

이 글에서는 퀵 정렬과 c언어의 표준 라이브러리에 존재하는 qsort() 함수에 대한 정보를 다룹니다.

* * *

### 퀵 정렬의 개념 및 특징

![img](https://blog.kakaocdn.net/dn/vlNwP/btqNfs4Csg1/jU7UOUBWIdVfltIaTf4EV0/img.gif)

퀵 정렬은 '분할 정복'이라는 개념에 바탕을 둔 알고리즘이다. 분할 정복은 전체를 공략하는 것이 아니라 전체를 일부로 나누어 공략하는 방식을 의미한다.

퀵 정렬은 '기준 요소(pivot) 선정 및 분할의 반복'이라는 과정을 통해 정렬을 진행한다. 자세한 과정은 아래와 같다.

&nbsp;**1.** 자료 구조에서 기준이 될 요소를 임의로 선정하고, 기준 요소(pivot)보다 작은 값을 가지는 요소는 기준 요소의 왼쪽으로, 큰 값을 가진 요소는 오른쪽으로 옮긴다.

&nbsp;**2.** 기준 요소 왼쪽에는 기준 요소보다 작은 요소의 그룹, 오른쪽에는 큰 요소의 그룹이 생기는데, 여기에서 왼쪽 그룹과 오른쪽 그룹을 분할하여 각 그룹에 대해 1의 과정을 수행한다.

&nbsp;**3.** 그룹의 크기가 1이하여서 더 이상 분할이 안 될때까지  1과 2의 과정을 반복하면 정렬이 완료된다.

&nbsp;

이러한 퀵 정렬은 시간복잡도 측면에서 다음과 같은 특징을 가진다.

**1.** 퀵 정렬은 기준 요소의 선정과 분할의 반복 때문에 **최선의 경우**(pivot이 배열의 중앙값이 되는 경우 등)엔 \*\*O(nlog<sub>2</sub>n)\*\*이라는 시간 복잡도를 가진다.

**2.** 하지만, **최악의 경우**(pivot을 배열의 최소값 혹은 최대값으로 잡았을 경우)에, \*\*(n-1)+(n-2)+(n-3)+...+1 = n(n-1)/2 = O(n<sup>2</sup>)\*\*이라는 시간 복잡도를 가지게 된다.

* * *

### 구현 과정

먼저 배열에서 두 원소의 위치를 바꿀 swap()함수가 필요하다.

그리고 배열을 반복을 통해 pivot보다 크고 작음을 기준으로 분할할 partition()함수가 필요하다.

마지막으로 이 과정을 재귀적으로 실행해주는 quicksort()함수가 필요하다.

* * *

### swap() 구현

```c
void swap(int* a, int* b){ //포인터로 받지 않으면 실제 값의 변화는 함수 내에서만 그치게 된다.
    int temp = *a;
    *a = *b;
    *b = temp;
}
```

&nbsp;swap()함수를 통해 두 변수의 값을 바꾼다.

* * *

### partition() 구현

```c
int partition(int data[], int left, int right){
    int first = left;
    int pivot = data[first];
    ++left;
    
    while(left <= right){
        while(data[left] <= pivot && left < right){ // left와 right가 만나면 반복 종료. 아래의 while문이 실행, 불필요한 비교x
            left++;
        }
        while(data[right] >= pivot && left <= right){ // right가 pivot값에도 도달할 수 있게 left<=right로 설정 
            right--;
        }
        if (left < right){ //left와 right가 교차되지 않으면, pivot보다 큰 값이 왼쪽에 있다는 것.
            swap(&data[left], &data[right]); 
        }
        else{
            break;
        }
    }
    swap(&data[first], &data[right]); //right가 pivot 위치에 도달하면 교환 발생x
    
    return right;
}
```

partition()함수를 통해 pivot값을 기준으로 작은 값은 왼쪽, 큰 값은 오른쪽에 정렬.

* * *

### quicksort() 구현

```c
void quicksort(int data[], int left, int right){
    if (left <= right){
        int index = partition(data, left, right);
        quicksort(data, left, index-1); // 재귀를 이용해 반복
        quicksort(data, index+1, right);
    }
}
```

quicksort()를 통해 재귀적으로 partition() 과정을 반복해서 정렬을 완료한다.

* * *

### 전체 구현 코드

```c
#include <stdio.h>

void swap(int* a, int* b){
    int temp = *a;
    *a = *b;
    *b = temp;
}

int partition(int data[], int left, int right){
    int first = left;
    int pivot = data[first];
    ++left;
    
    while(left <= right){
        while(data[left] <= pivot && left < right){ // left와 right가 만나면 반복 종료. 아래의 while문이 실행, 불필요한 비교x
            left++;
        }
        while(data[right] >= pivot && left <= right){ // right가 pivot값에도 도달할 수 있게 left<=right로 설정 
            right--;
        }
        if (left < right){ //left와 right가 교차되지 않으면, pivot보다 큰 값이 왼쪽에 있다는 것.
            swap(&data[left], &data[right]); 
        }
        else{
            break;
        }
    }
    swap(&data[first], &data[right]); //right가 pivot 위치에 도달하면 교환 발생x
    
    return right;
}

void quicksort(int data[], int left, int right){
    if (left <= right){
        int index = partition(data, left, right);
        quicksort(data, left, index-1);
        quicksort(data, index+1, right);
    }
}

int main() {
    int arr[] = {5,4,3,2,1};
    
    int length = sizeof(arr)/sizeof(arr[0]);
    
    quicksort(arr, 0, length-1);
    
    for(int i = 0; i < length; i++){
        printf("%d", arr[i]);
    }
    
    return 0;
}
```

**출력결과**

> **12345**

&nbsp;

**헷갈린 점:**

pivot보다 작은 값은 pivot의 왼쪽으로, 큰 값은 pivot의 오른쪽으로 옮긴다고 해서 실제로 왼쪽과 오른쪽에 다 나누어 옮기는 줄 알았는데,

구현 과정을 보면 그냥 위치를 변경하면서 정렬을 진행한다는 것을 알았다.

* * *

### c언어 표준 라이브러리의 qsort()

c언어 표준 라이브러리(stdlib.h)에서는 이러한 퀵 정렬을 제공하는 qsort()라는 함수를 사용할 수 있다.

* * *

### qsort()의 구조

qsort의 구조는 아래와 같다.

```c
void qsort(
    void* base, // 정렬 대상 배열의 주소
    size_t num, // 데이터 요소의 개수
    size_t width, // 개별 데이터 요소의 크기
    int(cdecl* compare)(const void *, const void *) // 비교 함수에 대한 포인터
);
```

base는 정렬할 대상의 배열 주소, num은 전체 배열의 크기, width는 배열 자료형의 개별 크기, 마지막은 비교 함수의 결과를 반환한다.

* * *

### 비교 함수(compare)의 구조

여기서 마지막의 매개 변수로 넘기는 포인터가 가리키는 함수는 아래의 구조를 가져야 한다.

`int compare((void*) & elem1, (void*) & elem2);`

이 함수를 정렬해야 하는 자료형에 맞춰 elem1과 elem2를 비교하는 코드를 작성해야 한다.

```c
int compare(const void*_elem1, const void*_elem2){
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

이렇게 elem1과 elem2를 비교해서 elem1이 더 클 경우엔 1, elem2가 더 클 경우엔 -1, 두 값이 같다면 0을 리턴하게 만들었다.

* * *

### qsort()를 활용한 전체 코드

qsort()를 활용한 전체 코드는 아래와 같다.

```c
#include <stdio.h>
#include <stdlib.h>

int compare(const void*_elem1, const void*_elem2){
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
    int data[] = {6,4,2,3,1,5};
    int length = sizeof(data)/sizeof(data[0]);
    int i = 0;
    
    qsort((void*)data, length, sizeof(int), compare);
    
    for (i = 0; i < length; i++){
        printf("%d", data[i]);
    }
    printf("\n");
    
    return 0;
}
```

**출력결과**

> &nbsp;**123456**

&nbsp;