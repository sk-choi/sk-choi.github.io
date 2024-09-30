---
title : "스택(링크드 리스트 기반) 개념 및 구현"
date : 2024-09-11 22:30:30 +/-TTTT
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

이 글은 링크드 리스트 기반의 스택에 대한 설명과 구현에 대한 정보를 담은 글입니다.

* * *

이전에는 [배열 기반의 스택 구조](https://sk-choi.github.io/data_structure/stack%28array_base%29/)를 구현했다면, 이번에는 링크드 리스트 기반의 스택을 구현해볼 것이다.

구현해 볼 스택의 구조는 및 기능은 다음과 같다.

1.  **노드와 스택 구조 선언**
2.  **스택 생성 기능**
3.  **스택 지우기 기능**
4.  **노드 생성 기능**
5.  **노드 지우기 기능**
6.  **Push**
7.  **Pop**
8.  **최상위 노드 반환**
9.  **비어있는지 확인**
10. **스택의 원소 개수 반환**

* * *

### 1\. 노드와 스택 구조 선언

```c
typedef struct tagNode{
    int data;
    struct tagNode* NextNode;
} Node; //노드 구조는 링크드 리스트와 동일하다.

typedef struct tagLLStack{
    Node* List; //List를 통해 링크드 리스트를 관리
    Node* Top; //최상위 노드는 Top을 통해 접근한다.
}LLStack;
```

&nbsp;

* * *

### 2\. 스택 생성 기능(createStack)

```c
void createStack(LLStack** Stack){
    
    // 스택을 자유 저장소에 저장
    (*Stack) = (LLStack*)malloc(sizeof(LLStack));
    (*Stack)->List = NULL;
    (*Stack)->Top = NULL;
}
```

malloc을 통해 Stack의 영역을 할당한다.

그리고 Stack의 List와 Top을 NULL로 초기화한다.

* * *

### 3\. 스택 지우기 기능(destroyStack)

```c
void destroyStack(LLStack* Stack){
    while(!isEmpty(Stack)){
        Node* popped = LLS_Pop(Stack);
        destroyNode(popped);
    }
    
    free(Stack);
}
```

Stack이 비어있는지 확인하는 isEmpty를 통해 비어 있지 않은 동안 스택에서 제거할 노드를 popped 노드에 할당해 그 노드를

destroyNode 기능을 이용해 지운다.

그리고 Stack을 메모리 해제한다.

* * *

### 4\. 노드 생성 기능

```c
Node* createNode(int new_data){
    Node* NewNode = (Node*)malloc(sizeof(Node));
    NewNode->data = new_data;
    
    NewNode->NextNode = NULL;
    
    return NewNode;
}
```

createNode를 통해 노드를 생성한다. 이 부분은 링크드 리스트에서 노드를 생성하는 것과 동일하다.

* * *

### 5\. 노드 지우기 기능

```c
void destroyNode(Node* _Node){
    free(_Node);
}
```

destroyNode를 통해 노드를 지우는 기능인데, 이 부분도 링크드 리스트의 부분과 동일하다.

* * *

### 6\. Push

```c
void LLS_Push(LLStack* Stack, Node* NewNode){
    if(Stack->List == NULL)
    {
        Stack->List = NewNode;
    }
    else{
        Stack->Top->NextNode = NewNode;
    }
    Stack->Top = NewNode;
}
```

스택에 원소를 삽입하는 Push기능을 구현한 코드이다. Stack의 리스트가 비어있을 경우,

스택의 리스트에 새 노드를 추가하고,  아닐 경우엔 스택의 최상단 노드의 다음 노드에 새 노드를 추가한다.

그리고 다시 스택의 최상단 노드로 새 노드를 정의해준다.

* * *

### 7\. Pop

```c
Node* LLS_Pop(LLStack* Stack){
    Node* TopNode = Stack->Top; //함수가 반환할 최상위 노드 저장
    
    if (Stack->List == Stack->Top){
        Stack->List = NULL;
        Stack->Top = NULL;
    }
    else{
        Node* CurrentTop = Stack->List;
        while(CurrentTop != NULL && CurrentTop->NextNode != Stack->Top){
            CurrentTop = CurrentTop->NextNode;
        }
        Stack->Top = CurrentTop;
        Stack->Top->NextNode = NULL;
    }
    
    return TopNode;
}
```

스택에서 최상단 노드를 반환하고 반환한 노드를 지우는 Pop기능을 구현한 코드이다.

먼저 TopNode 노드에 기존 스택의 최상위 노드를 저장한다.

스택의 List와 Top이 동일할 경우(스택에 원소가 하나 있을 경우) List와 Top을 NULL로 초기화한다.

동일하지 않은 경우엔, CurrentTop에 스택의 시작점(맨 아래)을 저장한다. 그리고 CurrentTop이 NULL이 아닐때까지,

그리고 CurrentTop 노드의 다음 노드가 Stack->Top(기존 스택의 Top)과 같지 않을 때까지 반복문을 실행해 CurrentTop의 위치를 이동한다.

`Stack->Top = CurrentTop`을 통해 최상단 노드의 위치를 변환하고, 그 다음노드를  NULL로 초기화한다.

마지막으로 기존 스택의 최상단 노드를 저장했던 TopNode를 반환하면 된다.

공부하다 든 의문점: 개인적으로 공부하면서 링크드 리스트 기반 스택의 Pop 함수의 경우, 스택이 비어있을 때 어떻게 동작할지에 대한 설명이 고려 안 된 것 같다. 이 코드대로 실행을 하면 결국 마지막 남은 1개의 데이터만 계속 반환이 된다.

이 부분을 고쳐 스택에 아무것도 없을 때는 -1을 반환하는 기능을 넣고 싶었지만, 아직 구현을 하진 못했다.

* * *

### 8\. 최상위 노드 반환

```c
Node* LLS_Top(LLStack* Stack){
    return Stack->Top;
}
```

최상위 노드를 반환하는 LLS_Top은 Stack->Top을 통해 최상위 노드를 반환하면 된다.

* * *

### 9\. 비어있는지 확인

```c
int LLS_isEmpty(LLStack* Stack){
    return (Stack->List == NULL);
}
```

&nbsp;스택이 비어있는지 확인하는 isEmpty는 `Stack->List == NULL`을 통해 스택이 비어있으면 1, 아니면 0을 반환한다.

* * *

### 10\. 스택의 원소 개수 반환

```c
int LLS_size(LLStack* Stack){
    int count = 0;
    Node* Current = Stack->List;
    
    while(Current != NULL){
        Current = Current->NextNode;
        count++;
    }
    return count;
}
```

Size를 통해서 스택의 원소 개수가 몇 개인지 반환하는 기능을 담당한다.

Current 노드가 NULL이 아닐 때까지 반복문을 돌리면서 Current가 이동한 횟수대로 Count변수를 1씩 증가시킨다.

그리고 반복문이 끝나면 Count를 반환한다.

* * *

### 전체 구현 코드

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct tagNode{
    int data;
    struct tagNode* NextNode;
} Node;

typedef struct tagLLStack{
    Node* List;
    Node* Top;
}LLStack;

void createStack(LLStack** Stack){
    
    // 스택을 자유 저장소에 저장
    (*Stack) = (LLStack*)malloc(sizeof(LLStack));
    (*Stack)->List = NULL;
    (*Stack)->Top = NULL;
}

int LLS_isEmpty(LLStack* Stack){
    return (Stack->List == NULL);
}

Node* LLS_Pop(LLStack* Stack){
    Node* TopNode = Stack->Top;
    
    if (Stack->List == Stack->Top){
        Stack->List = NULL;
        Stack->Top = NULL;
    }
    else{
        Node* CurrentTop = Stack->List;
        while(CurrentTop != NULL && CurrentTop->NextNode != Stack->Top){
            CurrentTop = CurrentTop->NextNode;
        }
        Stack->Top = CurrentTop;
        Stack->Top->NextNode = NULL;
    }
    
    return TopNode;
}

void destroyNode(Node* _Node){
    free(_Node);
}

void destroyStack(LLStack* Stack){
    while(LLS_isEmpty(Stack)==0){
        Node* popped = LLS_Pop(Stack);
        destroyNode(popped);
    }
    
    free(Stack);
}

Node* createNode(int new_data){
    Node* NewNode = (Node*)malloc(sizeof(Node));
    NewNode->data = new_data;
    
    NewNode->NextNode = NULL;
    
    return NewNode;
}

void LLS_Push(LLStack* Stack, Node* NewNode){
    if(Stack->List == NULL)
    {
        Stack->List = NewNode;
    }
    else{
        Stack->Top->NextNode = NewNode;
    }
    Stack->Top = NewNode;
}

Node* LLS_Top(LLStack* Stack){
    return Stack->Top;
}

int LLS_size(LLStack* Stack){
    int count = 0;
    Node* Current = Stack->List;
    
    while(Current != NULL){
        Current = Current->NextNode;
        count++;
    }
    return count;
}


int main(void){
    
    int i = 0;
    Node* Popped;
    
    LLStack* Stack;
    createStack(&Stack);
    
    LLS_Push(Stack, createNode(1));
    LLS_Push(Stack, createNode(2));
    LLS_Push(Stack, createNode(3));
    LLS_Push(Stack, createNode(4));
    
    printf("Size: %d, Top: %d\n", LLS_size(Stack), LLS_Top(Stack)->data);
    
    Popped = LLS_Pop(Stack);
    
    printf("Popped: %d\n", Popped->data);
    
    destroyNode(Popped);
    
    printf("Size: %d, Top: %d\n", LLS_size(Stack), LLS_Top(Stack)->data);
    
}
```

왜인지는 모르겠지만, Pop을 했음에도 원소의 개수를 출력하는 함수는 원래 크기를 반환한다.

이 부분을 어떻게 수정할 지 고민해봐야겠다.

**수정 사항** : 알고보니 createStack함수와 main함수에 오류가 있었다. 오류 사항은 다시 수정했고 결과도 정상적으로 출력되었다.

`createStack()`에서 `Stack->List = NULL`이 두 개 있어서 하나를 `Stack->Top = NULL`로 바꿔주었다.

**출력 결과:**

> Size: 4, Top: 4  
> Popped: 4  
> Size: 3, Top: 3

* * *

### 알게 된 점

배열의 경우 인덱스를 알고 있다면 시간복잡도를 O(1)수준으로 줄일 수 있지만, 링크드 리스트의 경우 그렇지 못하고, 일일이 원하는 위치까지 탐색을 진행하기 때문에 O(n)의 시간복잡도를 가질 수 밖에 없다. 이 점을 해결하기 위해서 Top 포인터를 통해 최상위 노드에 대한 접근 시간을 단축할 수 있다.