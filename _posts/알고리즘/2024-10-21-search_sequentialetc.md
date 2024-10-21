---
title : "탐색 알고리즘(순차 탐색, 전진 이동법, 전위법, 계수법)"
date : 2024-10-21 21:35:30 +/-TTTT
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

이 글은 탐색 알고리즘 중에서 순차 탐색, 전진 이동법, 전위법, 계수법에 대한 정보를 담은 글입니다.

* * *

### 순차탐색의 개념 및 특징

**개념:** 처음부터 끝까지 모든 요소를 검사하는 전략

**특징**: 똑똑하다기보다는 성실한 알고리즘이라 성능이 좋지는 않다. 구현이 간단해서 버그 적음

<details><summary>배열 순차 탐색 구현 코드 접기/펼치기</summary><div markdown="1">

```c
int sequential_search(int arr[], int target, int len){
    
    int match;
    for (int i = 0; i < len; i++){
        if (arr[i] == target){
            match = arr[i];
            return match;
        }
    }
    match = -1; // 없으면 -1리턴
    return match;
}
```

</div></details>

&nbsp;
* * *

### 자기구성 순차 탐색

<span style="color: #555555;">자주 사용되는 데이터를 데이터 집합의 앞쪽의 배치함으로써 순차 탐색을 효율을 끌어올리는 알고리즘</span>

**<span style="color: #555555;">종류: 전진이동법(move to front method), 전위법(transpose method), 빈도 계수법(frequency count method)</span>**

* * *

### **전진 이동법**

탐색된 항목을 데이터의 가장 앞으로 옮기는 방식, 한 번 찾은 항목을 다시 찾을 때 바로 탐색이 완료.

**링크드 리스트에서의 전진이동법**

<details><summary>링크드 리스트 전진 이동법 코드 접기/펼치기</summary><div markdown="1">

```c
Node* movetofront(Node** Head, int target){
    Node* Current = (*Head);
    Node* Previous = NULL;
    Node* Match = NULL;
    
    while(Current != NULL){
        if (Current->data == target){
            Match = Current;
            if (Previous != NULL){
                Previous->nextnode = Current->nextnode; // 자신의 이전 노드와 다음 노드를 연결?
                Current->nextnode = (*Head); // 자신을 리스트의 가장 앞으로 옮기기?
                (*Head) = Current;
            }
            break;
        }
        else{
            Previous = Current;
            Current = Current->nextnode;
        }
    }
    return Match;
}
```

</div></details>

&nbsp;

**배열에서의 전진 이동법**

<details><summary>배열 전진이동법 코드 접기/펼치기</summary><div markdown="1">

```c
int movetofront(int arr[], int target, int len){
    
    int match;
    
    int i = 0;
    while(i < len){
        if (arr[i] == target){
            if (i > 0){
                match = arr[i];
                memmove(&arr[1], &arr[0], sizeof(int)*i);
                arr[0] = match;
            }
            else{
                match = arr[0];
            }
            return match;
            break;
        }
        i++;
    }    
    return -1;
}
```
</div></details>

&nbsp;
* * *

### 전위법

탐색된 항목을 바로 이전 항목과 교환하는 전략을 취하는 알고리즘.

전진 이동법이 탐색된 요소를  무조건 앞으로 옮기는 것에 비해서 전위법은 자주 탐색된 알고리즘을 조금씩 앞으로 옮긴다.

결과적으로 자주 탐색되는 요소가 데이터의 앞에 모이게 됨.

**링크드 리스트에서의 전위법**

<details><summary>링크드 리스트 전위법 코드 접기/펼치기</summary><div markdown="1">

```c
Node* transpose(Node** Head, int target){
    
    Node* Current = (*Head);
    Node* PPrevious = NULL;
    Node* Previous = NULL;
    Node* match = NULL;
    
    while(Current != NULL){
        if (Current->data == target){
            match = Current;
            if (Previous != NULL){
                if (PPrevious != NULL){
                    PPrevious->nextnode = Current;
                }
                else{
                    (*Head) = Current;
                }
                Previous->nextnode = Current->nextnode;
                Current->nextnode = Previous;
            }
            break;
        }
        else{
            if (Previous != NULL){
                PPrevious = Previous;
            }
            Previous = Current;
            Current = Current->nextnode;
        }
    }
    return match;
}
```

</div></details>

&nbsp;  
**배열에서의 전위법**

<details><summary>배열 전위법 코드 접기/펼치기</summary><div markdown="1">

```c
int transpose(int arr[], int target, int len){
    
    int match;
    int i = 0;
    while(i < len){
        if (arr[i] == target){
            if(len > 1){
                match = arr[i];
                swap(&arr[i], &arr[i-1]);
            }
            else{
                match = arr[i];
            }
            return match;
            break;
        }
        i++;
    }
    return -1;
}
```

</div></details>

&nbsp;

* * *

### 계수법

데이터 내 각 요소가 탐색된 횟수를 별도의 공간에 저장을 하고, 탐색된 횟수가 높은 순으로 데이터를 재구성하는 전략의 알고리즘.

데이터의 수가 많을 경우, 앞에 위치하는 요소는 계속 앞에 위치하고, 뒤쪽에 위치하는 요소는 가장 앞으로 가기 힘든 전위법보다는 민주적인 방법.

그러나 계수 결과를 저장할 별도의 공간이 필요하고, 계수 결과에 따라 데이터를 재배치해야 하는 등의 비용 소모 발생.

<details><summary>배열 계수법 코드 접기/펼치기</summary><div markdown="1">

```c
#include <stdio.h>
#include <stdlib.h>

int max(int arr[], int len){
    
    int max = arr[0];
    for (int i = 1; i < len; i++){
        if (max < arr[i]){
            max = arr[i];
        }
    }
    return max;
}

int max_index(int arr[], int len){
    
    int max = arr[0];
    int maxindex = 0;
    for (int i = 0; i < len; i++){
        if (max < arr[i]){
            max = arr[i];
            maxindex = i;
        }
    }
    return maxindex;
}

int frequency_count(int arr[], int frq[], int target, int A_len){
    
    int match;
    int i = 0;
    while(i < A_len){
        if (arr[i] == target){
            match = arr[i];
            frq[arr[i]]++;
            return match;
        }
        i++;
    }
    return -1;
}

void sort_frequency(int a[], int c[], int frq[], int A_len){
    
    for(int i = 0; i < A_len; i++){
        int num = max_index(frq, max(a, A_len)+1);
        c[i] = num;
        frq[num] = 0;
    }
    memmove(&a[0], &c[0], sizeof(int)*A_len);
    //printf("%d\n", c[1]);
}

int main() {
    int A[] = {5,4,3,2,1};
    
    int A_len = sizeof(A)/sizeof(A[0]);
    
    int* B = (int*)calloc(max(A, A_len)+1, sizeof(int));

    for (int i = 0; i < A_len; i++){ // A에 있는 원소 체크하기 위해 1 할당(탐색을 하지 않은 원소도 보여주기 위해서)
        B[A[i]] = 1;
    }
    
    int* C= (int*)malloc(sizeof(int) * A_len);
    
    frequency_count(A, B, 2, A_len); //배열에서 2찾음
    frequency_count(A, B, 2, A_len); //배열에서 2찾음
    frequency_count(A, B, 5, A_len); //배열에서 5찾음
    frequency_count(A, B, 5, A_len); //배열에서 5찾음
    frequency_count(A, B, 1, A_len); //배열에서 1찾음
    frequency_count(A, B, 5, A_len); //배열에서 5찾음
    frequency_count(A, B, 4, A_len); //배열에서 4찾음
    frequency_count(A, B, 3, A_len); //배열에서 3찾음
    sort_frequency(A, C, B, A_len); // 찾은 빈도대로 A배열 C에 정렬

    for (int i = 0; i< A_len; i++){
        printf("%d", A[i]);
    }
}
```

</div></details>

&nbsp;