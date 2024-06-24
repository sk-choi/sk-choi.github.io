---
title : "더블 링크드 리스트(Double Linked List)개념 및 구현"
date : 2024-06-25 01:14:30 +/-TTTT
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

이 글은 더블 링크드 리스트에 대한 설명과 구현에 대한 정보를 담은 글입니다.

* * *

### 더블 링크드 리스트(Double Linked List)란?

<img src="data:image/png;base64,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" alt="img" width="742" height="151">

더블 링크드 리스트란 링크드 리스트의 탐색 기능을 개선한 자료구조이다.

단방향으로 노드를 연결한 링크드 리스트와 다르게(일반 링크드 리스트에 대한 정보는 [여기](https://sk-choi.github.io/data_structure/linkedList/)를 참고)

더블 링크드 리스트는 일반적인 링크드 리스트와 다르게 양방향으로 탐색이 가능하다.

그 이유는 더블 링크드 리스트가 이전 노드를 가리키는 포인터와 다음 노드를 가리키는 포인터를 모두 가지고 있기 때문이다.

더블 링크드 리스트의 주요 연산은 일반적인 링크드 리스트와 다른 것이 없다. 이전 노드를 처리하기 위한 과정이 추가될 뿐이다.

더블 링크드 리스트의 구현 과정은 아래와 같다.

* * *

### 더블 링크드 리스트 구현 과정

더블 링크드 리스트를 구현하기 위해서는 다음의 연산이 필요하다.

1.  **노드 생성(DLL_CreateNode)**
2.  **노드 소멸(DLL_DestroyNode)**
3.  **노드 추가(DLL_AppendNode)**
4.  **노드 탐색(DLL_GetNodeAt)**
5.  **노드 삭제(DLL_RemoveNode)**
6.  **노드 삽입(DLL_InsertAfter)**
7.  **노드 개수 세기(DLL_GetNodeCount)**

일단 연산 구현하기에 앞서서 더블 링크드 리스트의 노드 구조는 다음과 같다.

```c
// Node
typedef struct tagNode{
    int data; //숫자 자료형 저장
    struct tagNode* PreNode; //이전 노드를 카리켜서 구조체 포인터로 정의
    struct tagNode* NextNode;//다음 노드를 가리켜서 구조체 포인터로 정의
} Node;
```

더블 링크드 리스트는 양방향으로 탐색이 이루어지기에 PreNode와 NextNode에 대한 부분이 필요하다.

구조체를 PreNode와 NextNode로 가리키면서 서로 이어지게 하는 것이라 볼 수 있다.

* * *

### 1\. 노드 생성(DLL_CreateNode) 연산 구현

```c
// DLL_CreateNode
Node* DLL_CreateNode(int new_data){
    Node* NewNode = (Node*)malloc(sizeof(Node));
    NewNode->data = new_data;
    NewNode->PreNode = NULL;
    NewNode->NextNode = NULL;

    return NewNode;
}
```

DLL_CreateNode연산은 malloc을 통해서 공간을 할당하고 newdata를 data에, 이전노드와 다음노드를 가리키는 포인터는 NULL로 초기화를 한다.

* * *

### 2\. 노드 소멸(DLL_DestroyNode) 연산 구현

```c
// DLL_DestroyNode
void DLL_DestroyNode(Node* Node){
    free(Node);
}
```

&nbsp;노드 소멸은 free를 통해 메모지 할당 해제를 수행함으로써 실행한다.

* * *

### 3\. 노드 추가(DLL_AppendNode) 연산 구현

```c
// DLL_AppendNode
void DLL_AppendNode(Node** Head, Node* NewNode){
    if ((*Head) == NULL){ //Head가 가리키는 게 없으면 NewNode = Head
        (*Head) = NewNode;
    }
    else{ // Head가 있으면
        Node* Current = (*Head); //Current에 Head가 가리키는 주소 할당
        while(Current->NextNode != NULL){ //Current의 다음 노드가 NULL이 아닐때까지
            Current = Current->NextNode; //Current는 Current의 다음노드 지시
        }
        Current->NextNode = NewNode; //반복 완료하면 Current의 다음노드에 NewNode 담기
        NewNode->PreNode = Current; //NewNode의 이전 노드에 Current 할당(이중연결이라서)
    }
}
```

&nbsp;DLL_AppendNode는 노드를 기존의 리스트 끝(Tail) 바로 다음에 연결하는 연산을 수행한다.

리스트의 시작(Head)에 아무것도 없을 때는 추가하려는 노드가 Head가 되고,

기존에 추가된 노드가 있을 경우에는 반복을 통해 Tail에 도달해서 Tail의 NextNode에 추가할 노드를 연결하는 방식이다. 그리고 여기서 중요한 것은 추가한 노드의 PreNode에 기존의 Tail 노드(위 코드 상에선 Current)를 연결해야 한다는 것이다.

* * *

### 4\. 노드 탐색(DLL_GetNodeAt) 연산 구현

```c
//DLL_GetNodeAt
Node* DLL_GetNodeAt(Node* Head, int Location){
    
    Node* Current = Head; //Current에 Head 주소 할당
    while(Current != NULL && Location > 0){ //Current가 NULL이 아닐때까지 && Location이 0 이하인 동안 반복
        Current = Current->NextNode; //Current는 Current의 다음노드로 이동
        Location--; //Location에서 1씩 감소
    }
    return Current;
}
```

&nbsp;DLL_GetNodeAt 연산은 노드가 전체 리스트에서 몇 번째에 위치하는지 알아내는 기능을 수행한다. 0부터 셈을 시작하며 Location에 수를 입력하면 그 입력한 n번째 위치의 노드를 반환한다.

Current에 Head의 주소를 할당하고, Current가 NULL이 아닐 때까지, 그리고 Location이 0보다 클 때까지 반복을 수행한다. 결국 Location이 0이 되면 그만큼 다음 노드로 이동한 Current를 반환하는 것이다.

* * *

### 5\. 노드 삭제(DLL_RemoveNode) 연산 구현

```c
//DLL_RemoveNode
Node* DLL_RemoveNode(Node** Head, Node* Remove){
    if((*Head) == Remove){ //Head와 Remove가 같을 때
        *Head = Remove->NextNode; //Head는 Remove의 다음 노드 지시
        if ((*Head) != NULL) //만약 Head가 NULL이 아니라면
            (*Head)->PreNode = NULL; //Head의 이전 노드는 NULL
        Remove->PreNode = NULL; //Remove의 이전 노드는 NULL
        Remove->NextNode = NULL; //Remove의 다음 노드는 NULL
    }
    else{
        Node* Temp = Remove; // Temp에 Remove 할당
        Remove->PreNode->NextNode = Temp->NextNode; //Remove의 이전노드의 다음노드는 Temp의 다음 노드
        Remove->NextNode->PreNode = Temp->PreNode; //Remove의 다음노드의 이전노드는 Temp의 이전노드
    }
    Remove->PreNode = NULL; //Remove의 이전 노드는 NULL
    Remove->NextNode = NULL; //Remove의 다음 노드는 NULL
}
```

DLL_RemoveNode 연산은 노드를 삭제하는 연산을 수행한다. 여기서 노드를 삭제한다는 것은 기존에 리스트의 일부로서 다른 노드와 연결되어 있던 특정 노드의 연결을 해제하고, 그 노드를 제외한 다른 노드끼리 연결하도록 수정하는 작업을 의미한다.

이 작업을 수행하기 위해서, 일단 리스트의 시작점(Head)이 제거할 노드일 경우에 제거할 노드의 다음 노드를 Head로 재설정하고, 이렇게 재설정된 Head가 NULL이 아니라면 Head의 이전노드를 NULL로 선언함으로써, 새로운 Head를 온전한 Head로 만드는 과정을 수행한다.(시작점이기에 앞에 연결된 노드는 없기 때문이다.)

그리고 제거할 노드인 Remove의 앞 뒤 연결을 해제함으로써, Remove를 전체 리스트의 구성에서 제외해버린다.

만약 Remove가 Head가 아닐 경우엔 Temp에 Remove를 임시 할당하고, Remove 이전노드가 가리키는 다음 노드의 위치에 Remove의 다음 노드를 연결하고, Remove의 다음 노드가 가리키는 이전 노드의 위치에 Remove의 이전 노드를 연결한다. 이 과정을 그림으로 표현하면 다음과 같다.

<img src="https://github.com/sk-choi/sk-choi.github.io/assets/80041090/8a162738-fbdb-4c6b-9610-d11dcf53f26f" alt="더블링크드리스트_제거" width="529" height="429">  
간단히 표현하면 기존 연결 관계를 해제하고 새로운 연결을 만드는 과정이다.

* * *

### 6\. 노드 삽입(DLL_InsertAfter) 연산 구현

```c
//DLL_InsertAfter
Node* DLL_InsertAfter(Node* Current, Node* NewNode){
    NewNode->NextNode = Current->NextNode; // 새로운 노드의 다음 노드로 현재 노드의 다음 노드를 지시하기
    NewNode->PreNode = Current; // 새로운 노드의 이전 노드는 현재 노드

    if(Current->NextNode != NULL){ // 만약 현재 노드의 다음 노드가 존재하지 않는다면,
        Current->NextNode->PreNode = NewNode; // 현재 노드의 다음 노드가 지시하는 이전 노드로 새로운 노드를 지시하기
        Current->NextNode = NewNode; // 그리고 현재 노드의 다음 노드로 새로운 노드를 지시하기
    }
}
// 더블 링크드라서 두 방향 모두 작업이 필요하다.
```

DLL_InsertAfter 연산은 DLL_AppendNode 연산과는 다르게 현재 노드를 지정한 후, 그 노드의 다음에 새로운 노드를 삽입하는 연산을 수행한다.

즉 리스트의 처음, 중간, 마지막 등 순서 상관 없이 원하는 위치에 새로운 노드를 삽입하는 연산을 의미한다.

이 연산을 수행하기 위해서 우선 기존의 노드 연결을 재정의할 필요가 있다. 

기존의 노드 연결이 **"이전 노드"&lt;-&gt;"현재 노드"&lt;-&gt;"다음 노드"**라면 DLL_InsertAfter연산을 수행한 후에는

**"이전 노드"&lt;-&gt;"현재 노드"&lt;-&gt;"새로운 노드"&lt;-&gt;"다음 노드"** 이라는 새로운 연결 관계로 재구성 되는 것이다.

그래서 새로운 노드의 다음 노드엔 기존 현재 노드의 다음 노드가 위치하게 바꾸고, 새로운 노드의 이전 노드로는 현재 노드가 들어와야 한다.

만약 현재 노드의 다음 노드가 NULL이 아니라면(즉, 존재한다면), 현재 노드의 다음 노드가 지시하는 이전 노드가 원래는 Current이지만, 그 노드를 NewNode로 변경하고, 반대로 Current의 다음 노드를 NewNode로 변경하는 과정도 수행한다. (더블 링크드 리스트이기 때문에 역방향의 작업도 수행해야 하기 때문이다.)

* * *

### 7\. 노드 개수 세기(DLL_GetNodeCount) 연산 구현

```c
//DLL_GetNodeCount
int DLL_GetNodeCount(Node* Head){
    unsigned int count = 0;
    Node* Current = Head;
    while(Current != NULL){
        Current = Current->NextNode;
        count++;
    }
    return count;    
}
```

노드 개수 세기 연산인 DLL_GetNodeCount 연산은 앞의 연산보다 구현이 간단하다.

Current에 리스트의 시작점(Head)의 위치를 할당한 후, NULL이 나오지 않는 동안만 Current를 다음 노드로 이동하는 반복을 수행해 전체 노드의 개수를 파악하는 식으로 연산을 구현할 수 있다.

* * *

### 전체 구현 코드

전체를 구현한 코드는 아래와 같다.

```c
// Node
typedef struct tagNode{
    int data; //숫자 자료형 저장
    struct tagNode* PreNode; //이전 노드를 카리켜서 구조체포인터로 정의
    struct tagNode* NextNode;//다음 노드를 가리켜서 구조체포인터로 정의
} Node;

// DLL_CreateNode
Node* DLL_CreateNode(int new_data){
    Node* NewNode = (Node*)malloc(sizeof(Node));
    NewNode->data = new_data;
    NewNode->PreNode = NULL;
    NewNode->NextNode = NULL;

    return NewNode;
}

// DLL_DestroyNode
void DLL_DestroyNode(Node* Node){
    free(Node);
}

// DLL_AppendNode
void DLL_AppendNode(Node** Head, Node* NewNode){
    if ((*Head) == NULL){ //Head가 가리키는 게 없으면 NewNode = Head
        (*Head) = NewNode;
    }
    else{ // Head가 있으면
        Node* Current = (*Head); //Current에 Head가 가리키는 주소 할당
        while(Current->NextNode != NULL){ //Current의 다음 노드가 NULL이 아닐때까지
            Current = Current->NextNode; //Current는 Current의 다음노드 지시
        }
        Current->NextNode = NewNode; //반복 완료하면 Current의 다음노드에 NewNode 담기
        NewNode->PreNode = Current; //NewNode의 이전 노드에 Current 할당(이중연결이라서)
    }
}

//DLL_GetNodeAt
Node* DLL_GetNodeAt(Node* Head, int Location){
    
    Node* Current = Head; //Current에 Head 주소 할당
    while(Current != NULL && Location > 0){ //Current가 NULL이 아닐때까지 && Location이 0 이하인 동안 반복
        Current = Current->NextNode; //Current는 Current의 다음노드로 이동
        Location--; //Location에서 1씩 감소
    }
    return Current;
}

//DLL_RemoveNode
Node* DLL_RemoveNode(Node** Head, Node* Remove){
    if((*Head) == Remove){ //Head와 Remove가 같을 때
        *Head = Remove->NextNode; //Head는 Remove의 다음 노드 지시
        if ((*Head) != NULL) //만약 Head가 NULL이 아니라면
            (*Head)->PreNode = NULL; //Head의 이전 노드는 NULL
        Remove->PreNode = NULL; //Remove의 이전 노드는 NULL
        Remove->NextNode = NULL; //Remove의 다음 노드는 NULL
    }
    else{
        Node* Temp = Remove; // Temp에 Remove 할당
        Remove->PreNode->NextNode = Temp->NextNode; //Remove의 이전노드의 다음노드는 Temp의 다음 노드
        Remove->NextNode->PreNode = Temp->PreNode; //Remove의 다음노드의 이전노드는 Temp의 이전노드
    }
    Remove->PreNode = NULL; //Remove의 이전 노드는 NULL
    Remove->NextNode = NULL; //Remove의 다음 노드는 NULL
}

//DLL_InsertAfter
Node* DLL_InsertAfter(Node* Current, Node* NewNode){
    NewNode->NextNode = Current->NextNode;
    NewNode->PreNode = Current;

    if(Current->NextNode != NULL){
        Current->NextNode->PreNode = NewNode;
        Current->NextNode = NewNode;
    }
}

//DLL_GetNodeCount
int DLL_GetNodeCount(Node* Head){
    unsigned int count = 0;
    Node* Current = Head;
    while(Current != NULL){
        Current = Current->NextNode;
        count++;
    }
    return count;    
}

int main(void){

    int i = 0;
    int count = 0;
    Node* List = NULL;
    Node* NewNode = NULL;
    Node* Current = NULL;

    for (i = 0; i < 5; i++){
        NewNode = DLL_CreateNode(i);
        DLL_AppendNode(&List, NewNode);
    }

    count = DLL_GetNodeCount(List);
    for (i = 0; i < count; i++){
        Current = DLL_GetNodeAt(List, i);
        printf("List[%d] : %d\n", i, Current->data);
    }
}
```

&nbsp;