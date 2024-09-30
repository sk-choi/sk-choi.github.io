---
title : "큐(링크드 리스트 기반) 구현"
date : 2024-09-30 20:12:30 +/-TTTT
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

이 글은 링크드 리스트를 기반으로 한 큐를 설명하고 구현하는 정보를 담은 글입니다.

* * *

앞선 글에서 배열을 기반으로 한 [순환 큐](https://sk-choi.github.io/data_structure/queue_and_circularqueue/)를 구현한 바 있다.

배열을 기반으로 한 순환 큐는 한 번 정한 배열의 크기에 제한을 받는다는 단점이 존재한다.

이런 단점을 해결하기 위해 링크드 리스트를 활용하여 큐를 구현할 수 있다.

이렇게 링크드 리스트를 구현하면, 큐의 길이 상관 없이 Enqueue를 수행할 수 있다.

* * *

### 1\. 노드 구현과 큐 구현

```c
// 노드 구현
typedef struct tagNode{
    int data;
    struct tagNode* NextNode;
} Node;

// 큐 구현
typedef struct tagQueue{
    Node* Front;
    Node* Back;
    
} LLQueue;
```

노드 구현은 링크드 리스트를 구현할 때와 같다.

그리고 큐를 구현할 때 Front와 Back 포인터를 통해 바로 큐의 전단과 후단에 접근할 수 있게 한다.

* * *

### 2\. 노드 생성과 큐 생성 기능 구현

```c
// 노드 생성
Node* createNode(int data){
    Node* NewNode = (Node*)malloc(sizeof(Node));
    
    NewNode->data = data;
    NewNode->NextNode = NULL;
    
    return NewNode;
}

// 큐 생성
void createQueue(LLQueue** Queue){
    
    (*Queue) = (LLQueue*)malloc(sizeof(LLQueue));
    (*Queue)->Front = NULL;
    (*Queue)->Back = NULL;
}
```

&nbsp;노드와 큐 모두 malloc을 사용하여 메모리 할당을 해서 생성을 한다.

그리고 생성된 구조체 안의 변수들을 초기화한다.

* * *

### 3\. 노드 소멸과 큐 소멸 기능 구현 

```c
// 노드 소멸
void destroyNode(Node* _Node){
    free(_Node);
}

// 큐 소멸
void destroyQueue(LLQueue* Queue){
    while(!isEmpty(Queue)){
        Node* Popped = Dequeue(Queue);
        destroyNode(Popped);
    }
    free(Queue);
}
```

노드와 큐 모두 free를 사용해 메모리 할당을 해제한다.

큐의 메모리를 해제할 때에는 큐에 존재하는 노드들을 Dequeue한 후에 그 노드들을 메모리 해제하는 방식으로 진행한다.

그리고 마지막에 큐 자체의 메모리를 해제한다.

* * *

### 4\. 노드 삽입(Enqueue) 기능 구현

```c
void Enqueue(LLQueue* Queue, Node* NewNode){
    
    if (Queue->Front == NULL){
        Queue->Front = NewNode;
        Queue->Back = NewNode;
    }
    else{
        Queue->Back->NextNode = NewNode;
        Queue->Back = NewNode;
    }
}
```

&nbsp;노드 삽입을 위해선 노드가 비어있을 때와 비어있지 않을 때를 구분해서 동작을 설정한다.

노드가 비어있을 때는 Front와 Back에 모두 NewNode를 할당하고, 그렇지 않은 경우에는 기존의 Back의 다음 노드에 NewNode를 할당하고

다시 할당한 NewNode를 Back으로 지정하는 절차를 수행하게 만든다.

&nbsp;

* * *

### 5\. 노드  제거(Dequeue) 기능 구현

```c
Node* Dequeue(LLQueue* Queue){
    
    Node* Temp = Queue->Front;
    if (Queue->Front->NextNode == NULL){
        Queue->Front = NULL;
        Queue->Back = NULL;
    }
    else{
        Queue->Front = Queue->Front->NextNode;
    }
    return Temp;
}
```

&nbsp;노드 제거는 맨 앞의 노드가 제거되는 것이므로 Front의 위치를 Temp에 복사하고,

Front의 다음 노드가 없을 때 Front와 Back 모두 NULL로 초기화한다.

그렇지 않은 경우에는 Front의 다음 노드를 Front로 재지정한다.

그리고 마지막으로 Temp를 리턴한다.

* * *

### 6\. 비어있는지 확인(isEmpty)하는 기능 구현

```c
int isEmpty(LLQueue* Queue){
    return (Queue->Front == NULL);    
}
```

큐가 비어있는지 확인하기 위해선 큐의 Front가 NULL인지만 확인을 하면 된다.

* * *

### 전체 구현 코드

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct tagNode{
    int data;
    struct tagNode* NextNode;
} Node;

typedef struct tagQueue{
    Node* Front;
    Node* Back;
    
} LLQueue;

Node* createNode(int data){
    Node* NewNode = (Node*)malloc(sizeof(Node));
    
    NewNode->data = data;
    NewNode->NextNode = NULL;
    
    return NewNode;
}

void createQueue(LLQueue** Queue){
    
    (*Queue) = (LLQueue*)malloc(sizeof(LLQueue));
    (*Queue)->Front = NULL;
    (*Queue)->Back = NULL;
}

void destroyNode(Node* _Node){
    free(_Node);
}

void Enqueue(LLQueue* Queue, Node* NewNode){
    
    if (Queue->Front == NULL){
        Queue->Front = NewNode;
        Queue->Back = NewNode;
    }
    else{
        Queue->Back->NextNode = NewNode;
        Queue->Back = NewNode;
    }
}

Node* Dequeue(LLQueue* Queue){
    
    Node* Temp = Queue->Front;
    if (Queue->Front->NextNode == NULL){
        Queue->Front = NULL;
        Queue->Back = NULL;
    }
    else{
        Queue->Front = Queue->Front->NextNode;
    }
    return Temp;
}

int isEmpty(LLQueue* Queue){
    return (Queue->Front == NULL);    
}

void destroyQueue(LLQueue* Queue){
    while(!isEmpty(Queue)){
        Node* Popped = Dequeue(Queue);
        destroyNode(Popped);
    }
    free(Queue);
}

int main() {
    
    LLQueue* Queue;
    createQueue(&Queue);
    
    printf("원래 큐: ");
    for (int i = 0; i < 5; i++){
        Enqueue(Queue, createNode(i+1));
        printf("%d", Queue->Back->data);
    }
    printf("\n");
    
    for (int i = 0; i < 5; i++){
        Node* Popped = Dequeue(Queue);
        printf("Dequeue실행 %d번째 : %d\n", i+1, Popped->data);
        destroyNode(Popped);
    }
    
    printf("큐 비어있는지 확인 : %d", isEmpty(Queue));
    
}
```

**실행 결과**

> 원래 큐: 12345  
> Dequeue실행 1번째 : 1  
> Dequeue실행 2번째 : 2  
> Dequeue실행 3번째 : 3  
> Dequeue실행 4번째 : 4  
> Dequeue실행 5번째 : 5  
> 큐 비어있는지 확인 : 1

* * *

### 알 게 된 점

사실 성능으로 따지면 노드를 생성하고 삭제하기 위해 malloc과 free를 호출할 필요가 없는 순환 큐가 더 빠르다(일반 배열로도 구현이 가능하기 때문).

하지만 링크드 리스트를 기반으로 한 큐는 배열처럼 용량의 제한이 있는 것이 아니기 때문에 큐의 크기를 미리 정할 수 없는 상황에서 사용하기에 더 적합할 수 있다.

따라서 필요한 크기를 미리 정의할 수 있고, 고성능이 요구되는 상황에서는 순환큐를, 그렇지 않다면 링크드 리스트 기반의 큐를 사용하는 것이 좋다.

부가적으로 Queue 구조체에 `int count`를 넣어서 큐의 원소 개수를 바로 구하게 만들 수도 있다.

코드를 아주 약간만 수정하면 되니 금방 고칠 수 있을 것이다.

&nbsp;