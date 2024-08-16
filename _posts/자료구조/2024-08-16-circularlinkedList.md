---
title : "환형 링크드 리스트(Circular Linked List)개념 및 구현"
date : 2024-08-16 22:28:30 +/-TTTT
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

이 글은 환형 링크드 리스트에 대한 설명과 구현에 대한 정보를 담은 글입니다.

* * *

### 환형 링크드 리스트란?

![img](https://velog.velcdn.com/images%2Fcha-suyeon%2Fpost%2F7d47eb8c-6a3d-4b69-8517-c36e29694c33%2Fimage.png)

환형 링크드 리스트란 이름에서 알 수 있듯이 리스트가 원형으로 이어져 있다는 특징을 가지고 있다.  
이런 특징 덕분에 노드의 시작인 헤드와 노드의 끝인 헤드가 서로 이어져 있다.

"시작을 알면 끝을 알고 끝을 알면 시작을 알 수 있다."는 말에서 알 수 있듯이 환형 링크드 리스트에서

테일은 '헤드의 이전 노드'이며  헤드는 '테일의 이전 노드'라고 말할 수 있다.

* * *

### 환형 링크드 리스트 구현 과정

환형 링크드 리스트의 구현 과정에서 구현한 부분(기능)은 크게 8가지인데 집중적으로 살펴보아야 할 부분은 노드 추가와 노드 삭제에 관련한 부분이다.

노드 추가(CDL_AppendNode)기능의 경우 새로운 노드의 추가 뒤에 리스트의 헤드와 테일을 다시 연결해주는 과정이 필요하다.

노드 삭제(CDL_RemoveNode)기능의 경우 삭제할 노드의 기존 연결을 지우고 새롭게 연결될 노드 간의 연결을 재설정 해주는 과정이 필요하다.

각 기능 별 구현과 설명은 아래와 같다.

* * *

### 1\. 구조체 선언 및 노드 생성(CDL_CreateNode)기능 구현

```c
// 구조체(노드) 구현
typedef struct tagNode{
    int data;
    struct tagNode* PrevNode;
    struct tagNode* NextNode;
} Node;
```

```c
// 노드 생성 기능 구현
Node* CDL_CreateNode(int newdata){
    Node* NewNode = (Node*)malloc(sizeof(Node));
    NewNode->data = newdata;
    NewNode->PrevNode = NULL;
    NewNode->NextNode = NULL;
    
    return NewNode;
}
```

* * *

### 2\. 노드 소멸(CDL_DestroyNode) 기능 구현 

```c
void CDL_DestroyNode(Node* Node){
    free(Node);
}
```

free를 통해 메모리 해제 진행

* * *

### 3\. 노드 추가(CDL_AppendNode) 기능 구현 

```c
// 노드 리스트에 추가하는 기능
void CDL_AppendNode(Node** Head, Node* NewNode){
    
    if((*Head) == NULL){
        *Head = NewNode;
        (*Head)->NextNode = *Head;
        (*Head)->PrevNode = *Head;
    }
    else{
        Node* Tail = (*Head)->PrevNode;
        
        Tail->NextNode->PrevNode = NewNode;
        Tail->NextNode = NewNode;
        
        NewNode->NextNode = (*Head);
        NewNode->PrevNode = Tail;
    }
}
```

&nbsp;헤드에 노드가 없으면 새 노드를 헤드로 지정하고, 노드가 있다면 테일과 헤드 사이에 새 노드를 연결하는 작업을 진행한다.

헤드의 이전노드에 새 노드를 지정하고, 테일의 다음 노드로 새 노드를 진행하면서도, 반대로 새 노드의 다음 노드로 헤드를,

새 노드의 이전 노드로 테일을 지정하면 완전한 연결이 이루어진다.

* * *

###  4. 노드 삽입(CDL_InsertNode) 기능 구현

```c
// 노드 리스트 중간에 삽입하는 기능
void CDL_InsertNode(Node* Current, Node* NewNode){
    
    NewNode->NextNode = Current->NextNode;
    NewNode->PrevNode = Current;
    
    if ((Current->NextNode) != NULL){
        
        Current->NextNode->PrevNode = NewNode;
        Current->NextNode = NewNode;
    }
}
```

&nbsp;

중간에 새로 삽입할 노드의 다음 노드에 기준이 되는 현재 노드의 다음 노드를 지정하고,

새로운 노드의 이전 노드에는 현재 노드를 지정한다.

만약 현재 노드의 다음 노드가 NULL이 아니라면 현재 노드의 다음 노드의 이전 노드로 새 노드를 지정하고

현재 노드의 다음 노드로 새 노드를 지정한다.  

* * *

### 5\. 노드 제거(CDL_RemoveNode) 기능 구현

```c
//노드 제거 기능
void CDL_RemoveNode(Node** Head, Node* Remove){
    
    if ((*Head) == Remove){
        (*Head)->PrevNode->NextNode = Remove->NextNode;
        (*Head)->NextNode->PrevNode = Remove->PrevNode;
        (*Head) = Remove->NextNode;
        Remove->PrevNode = NULL;
        Remove->NextNode = NULL;
    }
    else{
        Remove->PrevNode->NextNode = Remove->NextNode;
        Remove->NextNode->PrevNode = Remove->PrevNode;
        
        Remove->PrevNode = NULL;
        Remove->NextNode = NULL;
    }
}
```

&nbsp;

헤드 노드가 제거할 노드라면 헤드 이전 노드(테일)의 다음 노드를 헤드의 다음 노드와 연결하고,

헤드의 다음 노드의 이전 노드로 헤드의 이전 노드(테일)를 연결한다.

그리고 헤드는 기존 헤드의 다음 노드로 새로 지정 해준다.

만약 헤드가 제거할 노드가 아니라면, 제거할 노드의 이전 노드의 다음 노드로 제거할 노드의 다음 노드를 연결해주고,

제거할 노드의 다음 노드의 이전 노드로 제거할 노드의 이전 노드를 연결해준다.

그리고 제거할 노드의 이전 노드와 다음 노드를 각각 NULL로 지정해서 기존 연결을 취소한다.  

* * *

### 6\. 노드 위치 탐색(CDL_SearchNode) 기능 구현

&nbsp;

```c
// 찾고자 하는 위치에 해당하는 노드 반환
Node* CDL_SearchNode(Node* Head, int Location){
    
    Node* Current = Head;
    
    while((Current != NULL) && (Location > 0)){
        Current = Current->NextNode;
        Location--;
    }
    return Current;
}
```

찾고자 하는 위치(순번)에 존재하는 노드를 리턴하는 기능이다.

반복문을 통해 찾고자 하는 위치에 도달할 때까지 현재 노드를 계속 다음 노드로 바꾸면서 탐색 작업을 진행하고

그 위치에 존재하는 노드를 반환하는 함수이다.

* * *

### 7\. 노드 개수 세기(CDL_CountNode) 기능 구현

```c
// 노드 개수 세기 기능
int CDL_CountNode(Node* Head){
    unsigned int count = 0;
    Node* Current = Head;
    
    while(Current != NULL){
        Current = Current->NextNode;
        count++;
        if (Current == Head){
            break;
        }
    }
    return count;
}
```

&nbsp;환형 링크드 리스트는 헤드와 테일이 서로 이어져 있는 구조이다보니,

반복문을 통해 현재 노드의 위치를 계속 다음 노드로 바꿔가는 방식으로는

리스트의 노드 개수를 완전히 파악하기가 어렵다.

그래서 Current와 Head가 일치하면 반복을 멈추도록 해서 온전한 개수를 파악하도록 구현을 했다.

이 부분이 좀 헷갈리는데, Current와 Head 모두 실제 값을 저장하고 있는 변수가 아닌 어떤 구조체를 지시하는 포인터 구조체라서

이 둘을 직접 비교하는 것은 가리키는 주소를 비교하는 것이라는 생각을 하게 되었다.

* * *

### 8\. 전체 노드 출력(CDL_PrintNode) 기능 구현 

```c
// 전체 노드 출력
void CDL_PrintNode(Node* Head){
    
    Node* Current = Head;
    
    if(Current == NULL){
        printf("There is not node");
    }
    else{
        Node* Start = Current;
        printf("%s", "Circular List printing result:\n");
        do{
          printf("%d", Current->data);
          Current = Current->NextNode;
        } while(Start != Current);
        printf("\n");
    }
}
```

PrintNode의 경우도 CountNode와 마찬가지로 헤드와 테일이 이어져 있는 구조를 고려해서 구현을 해야 한다.

따라서 Start에 Current를 할당해서 Current를 다음 노드로 이동 시켜도 나중에 한바퀴를 다 돌았을 때 시작 노드값과 비교할 수 있게 만들었다.

그래서 시작 노드와 현재노드가 일치하지 않을 동안만 반복이 구현되도록 코드를 작성했다.

* * *

### 전체 구현 코드

```c
#include <stdio.h>
#include <stdlib.h>

// struct declare
typedef struct tagNode{
    int data;
    struct tagNode* PrevNode;
    struct tagNode* NextNode;
} Node;

// create node
Node* CDL_CreateNode(int newdata){
    Node* NewNode = (Node*)malloc(sizeof(Node));
    NewNode->data = newdata;
    NewNode->PrevNode = NULL;
    NewNode->NextNode = NULL;
    
    return NewNode;
}

// destroy node
void CDL_DestroyNode(Node* Node){
    free(Node);
}

// append node
void CDL_AppendNode(Node** Head, Node* NewNode){
    
    if((*Head) == NULL){
        *Head = NewNode;
        (*Head)->NextNode = *Head;
        (*Head)->PrevNode = *Head;
    }
    else{
        Node* Tail = (*Head)->PrevNode;
        
        Tail->NextNode->PrevNode = NewNode;
        Tail->NextNode = NewNode;
        
        NewNode->NextNode = (*Head);
        NewNode->PrevNode = Tail;
    }
}

// insert node
void CDL_InsertNode(Node* Current, Node* NewNode){
    
    NewNode->NextNode = Current->NextNode;
    NewNode->PrevNode = Current;
    
    if ((Current->NextNode) != NULL){
        
        Current->NextNode->PrevNode = NewNode;
        Current->NextNode = NewNode;
    }
}

// remove node
void CDL_RemoveNode(Node** Head, Node* Remove){
    
    if ((*Head) == Remove){
        (*Head)->PrevNode->NextNode = Remove->NextNode;
        (*Head)->NextNode->PrevNode = Remove->PrevNode;
        (*Head) = Remove->NextNode;
        Remove->PrevNode = NULL;
        Remove->NextNode = NULL;
    }
    else{
        Remove->PrevNode->NextNode = Remove->NextNode;
        Remove->NextNode->PrevNode = Remove->PrevNode;
        
        Remove->PrevNode = NULL;
        Remove->NextNode = NULL;
    }
}

// search node
Node* CDL_SearchNode(Node* Head, int Location){
    
    Node* Current = Head;
    
    while((Current != NULL) && (Location > 0)){
        Current = Current->NextNode;
        Location--;
    }
    return Current;
}

// count node
int CDL_CountNode(Node* Head){
    unsigned int count = 0;
    Node* Current = Head;
    
    while(Current != NULL){
        Current = Current->NextNode;
        count++;
        if (Current == Head){
            break;
        }
    }
    return count;
}

// print node
void CDL_PrintNode(Node* Head){
    
    Node* Current = Head;
    
    if(Current == NULL){
        printf("There is not node");
    }
    else{
        Node* Start = Current;
        printf("%s", "Circular List printing result:\n");
        do{
          printf("%d", Current->data);
          Current = Current->NextNode;
        } while(Start != Current);
        printf("\n");
    }
}
    
int main(void){
    
    Node* List = NULL;
    Node* Current = NULL;
    // creating node 1 to 5
    for(int i = 1; i < 6; i++){
        CDL_AppendNode(&List, CDL_CreateNode(i));
    }
    // printing nodes
    CDL_PrintNode(List);
    
    // collect node to remove
    Current = CDL_SearchNode(List, 3);
    CDL_RemoveNode(&List, Current);
    
    // printing nodes added change
    CDL_PrintNode(List);
    CDL_AppendNode(&List, CDL_CreateNode(3));
    CDL_PrintNode(List);
    int num = CDL_CountNode(List);
    printf("%d",num);
    //CDL_DestroyNode(List);
}
```

&nbsp;