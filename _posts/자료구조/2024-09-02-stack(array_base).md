---
title : "스택(배열 기반) 개념 및 구현"
date : 2024-09-02 21:04:30 +/-TTTT
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

이 글은 배열 기반의 스택에 대한 설명과 구현에 대한 정보를 담은 글입니다.

* * *

### **스택이란?**

스택에 대한 정보는 앞의 글([스택과 큐](https://sk-choi.github.io/data_structure/datastruc01/))에서 설명한 바가 있다.

스택에 대해 다시 언급하자면 가장 마지막에 들어간 데이터가 제일 먼저 나오고(LIFO),

가장 먼저 들어간 데이터가 가장 나중에 나오는(FILO) 특징을 가진 자료구조라고 말할 수 있다.

배열 기반의 스택을 구현하기 위한 주요 기능은 다음과 같다.

1.  **삽입(Push)**
2.  **제거(Pop)**
3.  **스택 및 노드 생성/소멸**
4.  **노드 삽입**
5.  **노드 제거**

부가적으로 구현할 기능은 아래와 같다.

1.  **스택의 가장 위의 원소 반환**
2.  **스택의 사이즈 반환**
3.  **스택이 비어있는지 확인**
4.  **스택이 가득 차 있는지 확인**

구체적인 구현 코드는 아래와 같다.

* * *

### 1\. 노드 선언 및 스택 선언

```c
typedef struct tagNode{
    int data;
}Node;

typedef struct tagStack{
    int capacity;
    int top;
    Node* Nodes;
} ArrayStack;
```

노드 선언 부분은 앞의 리스트와 유사하다. 그리고 스택을 선언할 때 배열의 크기를 결정하는 capacity 변수가 필요하며,

top 변수를 통해 원소의 인덱스를 할당한다.

* * *

### 2\. 스택 생성(createStack)

```c
void createStack(ArrayStack** Stack, int capacity){
    (*Stack) = (ArrayStack*)malloc(sizeof(ArrayStack));
    (*Stack)->Nodes = (Node*)malloc(sizeof(Node)*capacity);

    (*Stack)->capacity = capacity;
    (*Stack)->top = -1;
}
```

createStack() 함수를 통해서 Stack에 malloc으로 capacity 곱하기 노드 만큼의 공간을 할당하고, Stack의 top을 -1로 초기화한다.

만약 capacity가 5라면, 다섯 개의 노드 만큼의 공간이 할당되는 것이다.

top을 -1로 초기화하는 이유는 배열에서 첫 번째 원소를 가리키는 인덱스가 0부터 시작하기 때문이다.

따라서 빈 Stack에 원소가 들어가면 첫 번째 원소가 0번째 인덱스를 할당 받게 하기 위해 -1로 초기화를 해 준다.

* * *

### 3\. 스택 삭제(destroyStack)

```c
void destroyStack(ArrayStack* Stack){
    free(Stack->Nodes);
    free(Stack);
}
```

destroyStack() 함수를 통해서 Stack이 가리키는 Nodes를 지우고, 그 다음에 Stack도 지움으로써 메모리 해제를 완료한다.

* * *

###  4.  노드 삽입(PushNode)

```c
void PushNode(ArrayStack* Stack, int new_data){
    Stack->top++;
    Stack->Nodes[Stack->top].data = new_data;
}
```

PushNode()를 통해 스택에 노드를 삽입한다.

Stack의 top을 1만큼 증가 시키고 Stack이 가리키는 Nodes의 top번째 data에 new_data를 삽입한다.

top이 1 증가해서 0이 되었으므로, 처음에 들어간 값은 Stack의 0번째 노드에 할당이 된다.

* * *

### 5\. 노드 제거(PopNode)

```c
int PopNode(ArrayStack* Stack){
    int position = Stack->top--;
    return Stack->Nodes[position].data;
}
```

PopNode()를 통해 스택의 노드를 제거한다.

position변수를 선언해서 Stack의 top변수에 1을 뺀 값을 후위 증감 연산자으로 선언한다.

후위 표기로 선언하는 이유는 position변수 값에 top의 원래 값이 할당된 다음 top의 값을 1만큼 감소 시키기 때문에

해당 연산이 끝난 후 top은 1만큼 감소하게 된다.

만약 스택의 크기가 4이고, 이 스택이 가득 차 있을 경우에 PopNode()를 실행하면, top이 4이므로 4번째 원소가 출력 되고

top의 값이 3으로 수정되는 것이다.

* * *

### 6\. 스택의 가장 위의 원소 반환(TopNode)

```c
int TopNode(ArrayStack* Stack){
    return Stack->Nodes[Stack->top].data;
}
```

TopNode() 함수를 통해 Stack의 가장 위 원소를 반환할 수 있다.

Nodes의 top번째 원소를 반환하는 코드를 통해 가장 위의 원소를 반환한다.

* * *

### 7\. 스택의 사이즈 반환(SizeStack)

```c
int SizeStack(ArrayStack* Stack){
    return Stack->top+1;
}
```

SizeStack()을 통해서 Stack의 사이즈를 반환한다.

top+1이 Stack의 크기이므로 이 값을 반환하도록 한다.

* * *

### 8\. 스택이 비어있는지 확인(isEmptyStack)

```c
int isEmptyStack(ArrayStack* Stack){
    if(Stack->top == -1){
        return 1;
    }
    else{
        return 0;
    }
}
```

스택이 비어있는지 확인하기 위해서 top의 값이 -1인지 아닌 지를 조건문으로 판단한다.

top의 값이 -1이면 1을 반환해 스택이 비어있다고 알려주고, 아닐 경우 0을 반환해 비어있지 않다고 알려준다.

* * *

### 9\. 스택이 가득 차 있는지 확인(isFullStack)

```c
int isFullStack(ArrayStack* Stack){
    if(Stack->top == Stack->capacity-1){
        return 1;
    }
    else{
        return 0;
    }
}
```

스택이 가득 차 있는지 확인하는 방법은 isEmptyStack()과 반대로 top의 값이 capacity-1과 일치하는지 조건문으로 따지는 것이다.

capacity는 배열의 크기를 결정하는 변수이고 만약 capacity가 5일 경우, 배열의 최대 인덱스는 5에서 1을 뺀 값인 4일 것이므로

top의 값이 capacity-1과 일치하는 지를 따져야 한다.

그리고 일치할 경우 1을 반환하고, 아닐 경우 0을 반환하게 한다.

* * *

### 전체 구현 코드

전체 구현 코드는 아래와 같다.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct tagNode{
    int data;
}Node;

typedef struct tagStack{
    int capacity;
    int top;
    Node* Nodes;
} ArrayStack;

void createStack(ArrayStack** Stack, int capacity){
    (*Stack) = (ArrayStack*)malloc(sizeof(ArrayStack));
    (*Stack)->Nodes = (Node*)malloc(sizeof(Node)*capacity);

    (*Stack)->capacity = capacity;
    (*Stack)->top = -1;
}

void destroyStack(ArrayStack* Stack){
    free(Stack->Nodes);
    free(Stack);
}

void PushNode(ArrayStack* Stack, int new_data){
    Stack->top++;
    Stack->Nodes[Stack->top].data = new_data;
}

int PopNode(ArrayStack* Stack){
    int position = Stack->top--;
    return Stack->Nodes[position].data;
}

int TopNode(ArrayStack* Stack){
    return Stack->Nodes[Stack->top].data;
}

int SizeStack(ArrayStack* Stack){
    return Stack->top+1;
}

int isEmptyStack(ArrayStack* Stack){
    if(Stack->top == -1){
        return 1;
    }
    else{
        return 0;
    }
}

int isFullStack(ArrayStack* Stack){
    if(Stack->top == Stack->capacity-1){
        return 1;
    }
    else{
        return 0;
    }
}

int main(){
    
    int i = 0;
    ArrayStack* Stack = NULL;
    
    createStack(&Stack, 5);
    
    for(int i = 0; i < 5; i++){
        PushNode(Stack, i);
    }

    printf("TopNode: %d\n", TopNode(Stack));
    printf("Stack size: %d\n", SizeStack(Stack));
    printf("Empty?: %d\n", isEmptyStack(Stack));
    printf("Full?: %d\n", isFullStack(Stack));
    
    for(int i = 0; i < 5; i++){
        printf("%d", PopNode(Stack));
    }
    printf("\n");

    
    printf("Empty?: %d\n", isEmptyStack(Stack)); //비어 있으면 1, 아니면 0
    printf("Full?: %d\n", isFullStack(Stack));

    
    destroyStack(Stack);
}
```

&nbsp;