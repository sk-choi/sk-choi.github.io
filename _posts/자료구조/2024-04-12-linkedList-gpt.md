---
title : "링크드 리스트 구현과 관련한 지피티의 코드 리뷰"
date : 2024-04-12 23:51:30 +/-TTTT
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
---


링크드 리스트 구현과 관련해서

출력이 되지 않는 오류가 발생해서 지피티에게 코드 리뷰를 부탁했다.

&nbsp;c언어로 코드를 작성했고,

함수로는 리스트 노드를 만드는 `create_Node()`, 노드를 뒤에 삽입하는 `insert()`, 노드를 삭제하는 `delete()`,

연결리스트를 출력하는 `print_Node()`함수를 구현했다.

* * *

### 내가 작성한 코드

```c
#include <stdio.h>
#include <stdlib.h>

// 노드 구조체 선언 
typedef struct tagNode{
    int data;
    struct tagNode* next;
}Node;

// 노드 생성하는 함수
Node* create_Node(int data){
    Node* NewNode = (Node*)malloc(sizeof(Node)); //malloc을 통한 동적할당
    NewNode->data = data; //NewNode의 data에 입력값 할당
    NewNode->next = NULL; //노드의 초기 상태이므로 다음 노드를 지정하는 포인터에는 NULL할당

    return NewNode;
}

// 노드를 추가 연결하는 함수
void insert(Node** head, Node* newNode){
    if (*head == NULL) //헤드에 연결된 노드가 없다면
        *head = newNode; //newNode가 헤드가 된다.
    else{ // 연결된 노드가 있다면
        Node* current = *head; //current에 head값 할당
        while(current!=NULL){ //current가 NUll이 아닐때까지
            current = current->next; //current는 current 다음 노드로 이동
        }
        current = newNode->next; //current는 newNode의 다음 노드
    }
}

// 리스트를 삭제하는 함수
void delete(Node** head){
    free(*head); //free를 통한 메모리 해제
}

// 리스트를 출력하는 함수
void print_Node(Node **head){
    Node *current = *head; //current에 head값 할당
    while(current != NULL){ //current가 NULL이 아닐 때까지 반복
        printf("%d",current->data); //current가 지시하는 data 출력
    }
    current = current->next; //current를 다음 노드로 이동
}

int main() {

    Node** List = NULL;
    Node* new = create_Node(3);
    insert(List, new);
    print_Node(List);
        
    return 0;
}
```

&nbsp;코드를 실행했는데 출력이 되지 않아서 문제점을 짚어주도록 지피티에게 요청했다.

* * *

### 지피티가 말해준 문제점과 개선 코드

&nbsp;

지피티는 다음과 같은 점을 말해주었다.

> 네 코드를 살펴보겠습니다. 몇 가지 문제가 있네요:

1.  > `insert` 함수에서 새로운 노드를 연결 리스트에 삽입하는 부분이 잘못되었습니다. `current`를 새로운 노드의 `next`로 설정하는 대신, `current`가 마지막 노드를 가리키도록 해야 합니다.
    
2.  > `delete` 함수에서는 연결 리스트의 노드를 삭제하는데, 현재 구현은 헤드 노드만 삭제하고 나머지 노드들은 해제하지 않습니다.
    
3.  > `print_Node` 함수에서도 일부 문제가 있습니다. 노드를 이동시키는 부분이 빠져있어서 무한루프에 빠질 수 있습니다.
    
4.  > `main` 함수에서 연결 리스트의 헤드를 가리키는 이중 포인터인 `List`를 초기화하지 않았습니다.
    

* * *

### 지피티가 말한 문제점을 개선한 코드

&nbsp;

지피티가 말한 문제점을 인식하고 이를 반영하여 코드를 조금 수정해보았다.

```c
#include <stdio.h>
#include <stdlib.h>

//Node 구조체 선언
typedef struct tagNode{
    int data;
    struct tagNode* next;
}Node;

// Node 만드는 함수
Node* create_Node(int data){
    Node* NewNode = (Node*)malloc(sizeof(Node)); // 동적할당
    NewNode->data = data; // data값 할당
    NewNode->next = NULL; // Node 초기화 상태이므로 다음 노드에 연결은 NULL로 선언

    return NewNode;
}

//노드를 추가 삽입하는 함수
void insert(Node** head, Node* newNode){
    if (*head == NULL) //헤드에 연결된 노드가 없다면
        *head = newNode; // newNode를 헤드에 할당
    else{ // 헤드에 연결된 노드가 있다면
        Node* current = *head; // current에 head가 지시하는 주소 할당
        while(current->next != NULL){ // current의 다음 노드가 없을 때까지
            current = current->next; // current에 current 다음 노드 할당
        }
        current->next = newNode; // current의 뒤에 더 이상 노드가 없다면 newNode를 다음노드로 지정
    }
}

// 리스트를 지우는 함수
void delete(Node** head){
    free(*head); // free함수로 메모리 해제
    *head = NULL; // 헤드가 가리키는 노드도 초기화
}

// 리스트에 있는 노드를 모두 출력하는 함수
void print_Node(Node **head){
    Node *current = *head; // current에 헤드가 지시하는 주소 할당
    while(current != NULL){ // current 다음 노드가 없을 때까지
        printf("%d",current->data); // current가 가리키는 data값 출력
        current = current->next; // current에 다음 노드 할당, 다음으로 이동
    }
    printf("\n");
}

int main() {

    Node* List = NULL; //List 초기화
    Node* new = create_Node(3); // data가 3인 노드 만들기
    insert(&List, new); // 만든 노드를 리스트에 삽입
    Node* new02 = create_Node(4); // data가 4인 노드 만들기
    insert(&List, new02); // 만든 노드를 리스트에 삽입
    print_Node(&List); // 리스트 출력

    delete(&List); // 리스트 지우기
        
    return 0;
}
```

&nbsp;그리고 실행을 해보니 잘 돌아가는 것을 확인할 수 있었다.

  
![연결리스트_출력](https://github.com/sk-choi/sk-choi.github.io/assets/80041090/ef0d228a-eb61-4708-b26a-59b19942f6e6)