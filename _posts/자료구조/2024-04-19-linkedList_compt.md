---
title : "링크드 리스트 완전 구현(C언어)"
date : 2024-04-19 19:01:30 +/-TTTT
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

이번에 작성한 링크드 리스트 포스트는

저번에 작성하지 않았던  노드를 탐색하는 함수(search), 새 노드를 중간에 삽입하는 함수(insert), 노드 개수를 세는 함수(count),

삭제 노드를 입력받아 이전노드에 삭제 노드 다음의 노드를 연결하는 함수(remove)를 구현하였다.

저번에는 리스트 뒤에 새 노드를 삽입하는 함수 이름을 insert로 지었지만, 새 노드를 중간에 삽입하는 함수의 이름에 insert가 더 어울리는 것 같아서

기존의 insert함수를 append 함수로 수정했다.

* * *

### searchNode 함수 구현

```c
// 노드를 탐색하는 함수, 입력한 숫자의 위치에 해당하는 노드의 주소 반환
Node* searchNode(Node* head, int location){
    Node* current = head;

    while(current != NULL && location > 0){
        current = current->next; //current->next = current는 제자리 반복이니 헷갈리지 말자
        location--;
    }
    return current; // 포인터 반환
}
```

n번째 노드를 찾기 위해 location을 통해서 위치를 입력 받는다. (시작은 0부터)

현재 노드가 NULL이 아닐 때까지, 그리고 location이 0보다 클 때까지 반복을 한다.

조건을 만족하는 동안 반복을 해서 찾고자 하는 위치의 노드의 주소를 반환한다.

* * *

### Insert 함수 구현 

```c
// 새로 만든 노드를 중간에 삽입하는 함수
void insert(Node* current, Node* newNode){
    newNode->next = current->next; // 새 노드의 다음노드를 현재 위치의 노드의 다음노드로 바꾸기
    current->next = newNode; // 현재 위치의 노드의 다음 노드를 새로운 노드로 바꾸기
}
```

새 노드를 원래 현재 노드의 다음 노드로 넣고

원래 현재 노드의 다음 노드가 가리키던 곳에 새 노드의 주소를 넣는다.

* * *

###  count_Node 함수 구현

```c
// 노드 개수를 세는 함수
int count_Node(Node* Head){
    int count = 0;
    Node* current = Head;

    while(current != NULL){
        current = current->next;
        count++;
    }
    return count;
}
```

&nbsp;현재 노드가 NULL이 아닐때까지 반복해서

개수를 세어 반환한다.

* * *

### remove_Node 함수 구현 

```c
void remove_Node(Node** head, Node* remove){
    if(*head == remove){
        *head = remove->next; //헤드를 제거하므로 헤드 다음의 노드를 헤드로 임명
    }
    else{
        Node* current = *head;
        while(current != NULL && current->next != remove){
            current = current->next;
        }
        if (current != NULL){
            current->next = remove->next;
        }
    }
}
```

&nbsp;리스트에 있는 노드 중에서 특정 노드를 삭제하는 함수인데

현재 노드가 NULL이 아니고 현재 노드의 다음 노드가 삭제할 노드가 아닐 동안 반복해서 

삭제할 노드의 위치에 접근한다.

만약에 현재 노드가 NULL값이 아니라면

현재 노드의 다음 노드를 삭제한 노드의 다음 노드로 갱신한다.

* * *

### 최종 코드

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

//노드를 리스트 뒤에 추가 삽입하는 함수
void append(Node** head, Node* newNode){
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
void delete(Node* Node){
    free(Node); // free함수로 메모리 해제
}

// 노드를 탐색하는 함수, 입력한 숫자의 위치에 해당하는 노드의 주소 반환
Node* searchNode(Node* head, int location){
    Node* current = head;

    while(current != NULL && location > 0){
        current = current->next; //current->next = current는 제자리
        location--;
    }
    return current;
}

// 새로 만든 노드를 중간에 삽입하는 함수
void insert(Node* current, Node* newNode){
    newNode->next = current->next; // 새 노드의 다음노드를 현재 위치의 노드의 다음노드로 바꾸기
    current->next = newNode; // 현재 위치의 노드의 다음 노드를 새로운 노드로 바꾸기
}

// 노드 개수를 세는 함수
int count_Node(Node* Head){
    int count = 0;
    Node* current = Head;

    while(current != NULL){
        current = current->next;
        count++;
    }
    return count;
}

// 지울 노드를 이전 노드와 연결을 끊고 이전 노드를 지울 노드 다음 노드와 연결하는 함수
void remove_Node(Node** head, Node* remove){
    if(*head == remove){
        *head = remove->next; //헤드를 제거하므로 헤드 다음의 노드를 헤드로 임명
    }
    else{
        Node* current = *head;
        while(current != NULL && current->next != remove){
            current = current->next;
        }
        if (current != NULL){
            current->next = remove->next;
        }
    }
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
    Node* MyNode = NULL;

    append(&List, create_Node(117));
    append(&List, create_Node(119));
    append(&List, create_Node(212));

    MyNode = searchNode(List, 0);
    printf("%d\n", MyNode->data);
    remove_Node(&List, MyNode);
    delete(MyNode);
    
    return 0;
}
```

&nbsp;

&nbsp;

&nbsp;