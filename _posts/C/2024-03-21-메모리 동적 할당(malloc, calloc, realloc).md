---
title : "메모리 동적 할당(malloc, calloc, realloc)"
date : 2024-03-21 22:48:30 +/-TTTT
categories : 
- C
tags : 
- [C언어] #소문자만 가능
published : true
#permalink : categories/algorithm

header :
  teaser : https://i.namu.wiki/i/KcqDuQYTxNpUcLIMZTg28QXse0XiWx1G7K68kYYCo1GuhoHmhB_V8Qe9odGGt0BH9-0nQZTN53WXTNpDmwVfWQ.svg

toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
---


### Intro

이 글은 C언어의 메모리 동적 할당 함수 malloc, calloc, realloc 함수에 대해서 다룬다.

* * *

### 메모리 동적 할당이란?

사실 이 개념은 예전에 학교에서 C++수업을 처음 들었을 때 알게 된 것이다.

그 때 교수님의 말로는 메모리를  힙(heap)이라는 특수한 영역에 선언하여

메모리를 할당하는 것이라고 대충 들었는데, 사실 이 개념이 왜 필요하고 어떻게 쓰이는지는

잘 알기가 어려웠다.

우선, 메모리 동적 할당이란 자동 저장공간이 아닌 heap 영역에 메모리를 할당하는 것을 말한다.

그리고 C언어에서는 malloc, calloc, realloc 함수로 동적 할당을 할 수 있다.

* * *

### malloc & free

프로그램 실행 중에 메모리를 동적 할당할 때는 malloc, 그리고 할당한 메모리를 해지 및 반환할 때는

free함수를 사용한다.

그리고 malloc, free 함수를 사용하기 위해선 &lt;stdlib.h&gt;라는 헤더 파일을 선언해야 한다.

malloc함수와 free함수의 선언은 다음과 같다.

```c
int *pi;

pi = (int *)malloc(sizeof(int));

free(pi);
```

&nbsp;

```c
// malloc, free 함수 사용 예시

#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *pi;
    double *pd;
    
    pi = (int*)malloc(sizeof(int));
    if (pi == NULL)
    {
        printf("#메모리가 부족합니다.\n");
        exit(1);
    }
    pd = (double *)malloc(sizeof(double));
    
    *pi = 10;
    *pd = 3.4;
    
    printf("정수형으로 사용 : %d\n", *pi);
    printf("실수형으로 사용 : %.1lf\n", *pd);
    
    free(pi);
    free(pd);
    
    return 0;
}
```

&nbsp;

추가적으로 malloc 함수와 free 함수의 원형은 다음과 같다.

```c
void *malloc(unsigned int size);
void free(void *p);
```

&nbsp;malloc 함수의 반환형이 void \*라는 점에서 malloc 함수는 형변환을 통해

반환하는 자료형의 타입을 변경할 수 있는 함수라는 것을 알 수 있다.

* * *

###  주의점

1\. malloc 함수의 반환값이 널 포인터인지 확인해야 한다.

malloc 함수는 원하는 크기의 공간을 할당하지 못할 경우 널 포인터를 반환하기 때문에

if문 등을 통해서 널 포인터를 반환하는 경우에 프로그램을 종료하도록 설계해야 한다.

&nbsp;

2\. 사용이 끝난 저장 공간은 반환해야 한다.

동적으로 할당한 저장 공간은 함수가 반환되어도 계속 메모리에 남아 있기 때문에 사용이 끝나면 바로 free 함수를 통해 반환을 해야 한다. 그래야 반환한 메모리를 다시 재사용할 수 있다.

* * *

### calloc & realloc

calloc 함수는 malloc과는 다르게 할당한 공간을 0으로 초기화한다.

malloc 함수를 통해 동적으로 저장 공간으로 할당할 경우엔 처음에는 쓰레기 값으로 

할당 공간이 초기화 되어 있지만 calloc 함수는 0으로 초기화 한다.

calloc 함수의 원형은 다음과 같다.

```c
void *calloc(unsigned int, unsigned int);
```

&nbsp;

realloc 함수는 저장 공간의 크기를 조절할 수 있다. realloc 함수의 원형은 다음과 같다.

```c
void *realloc(void *, unsigned int);
```

&nbsp;

calloc & realloc 사용 예시

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *pi;
    int size = 5;
    int count = 0;
    
    int num;
    int i;
    
    pi = (int *)calloc(size, sizeof(int));
    while(1)
    {
        printf("양수만 입력하세요 => ");
        scanf("%d", &num);
        if (num <= 0) break;
        if (count == size)
        {
            size += 5;
            pi = (int *)malloc(pi, size * sizeof(int));
        }
        pi[count++] = num;
    }
    for (i = 0; i < count; i++)
    {
        printf("%5d", pi[i]);
    }
    free(pi);
    
    return 0;
}
```

&nbsp;

* * *

###  정리

|     |     |     |
| --- | --- | --- |
| 함수  | 사용 예시 | 기능  |
| malloc | int \*pi = (int \*)malloc(sizeof(int)) | size크기만큼 할당하고 시작 위치 반환 |
| calloc | double \*pi = (double \*)calloc(5, sizeof(double)); | (count \* size)만큼 할당하고 0으로 초기화 후 시작 위치 반환 |
| realloc | char \*pi = (char \*)realloc(p, 2 \* strlen(str)); | pi가 연결한 영역 크기를 size 크기로 조정하고 시작 위치 반환 |
| free | void free(void \*pi); | pi가 연결한 영역 반환 |

&nbsp;

&nbsp;

&nbsp;