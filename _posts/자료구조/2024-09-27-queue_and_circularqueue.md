---
title : "큐(Queue)의 개념 및 순환 큐 구현"
date : 2024-09-27 02:02:30 +/-TTTT
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

이 글은 큐에 대한 개념 설명과 순환 큐를 구현하는 정보를 담은 글입니다.

* * *

### 1\. 큐(Queue)의 개념

큐에 대한 간단한 개념과 설명은 앞의 글 [스택과 큐](https://sk-choi.github.io/data_structure/datastruc01/)에서 설명한 바 있다.

큐는 먼저 온 순서대로 처리하는 구조를 가진다. 은행창구에서 고객을 상대하거나 식당에서 줄을 세우는 것처럼

먼저 들어온 것이 먼저 나가는 FIFO 구조를 가지고 있다.

큐의 대표적인 예시로는 버퍼(buffer)가 있다.

* * *

### 2\. 큐의 구조

<img src="https://i.namu.wiki/i/sbq2-2lcGywLFhPbR4pWckz3LAZMf7xEtkB817i21jqSzk5XVdTGt0H1waIhXWSFvlBbj-7fvyfm-10dRpY6sg.webp" width="532" height="348" class="jop-noMdConv">

위 그림처럼 큐는 먼저 온 데이터가 삭제되는(나가는) 전단(Front)과 새로운 데이터가 들어오는 후단(Back 또는 Rear)으로 구성되어있고,

여기서 데이터가 나가는 것을 Dequeue, 새로 들어오는 것을 Enqueue라고 한다.

* * *

### 3\. 배열을 사용한 기초적인 구현과 단점

그렇다면 이러한 큐를 어떻게 구현을 할 수 있을까? 바로 배열을 사용하면 된다.

하지만 배열을 사용하면 Enqueue와 Dequeue를 반복할 수록 배열의 공간이 부족해진다는 단점이 발생한다.

<details open="open"><summary>간단 구현 코드 접기/펼치기</summary><div markdown="1">

```c
// 배열을 사용한 간단한 큐 구현 (기능은 Deque만 구현)  
#include <stdio.h>

void Deque(int arr[]){  
    for(int i = 0; i < 5; i++){  
        if (arr[i] != 0){  
            arr[i] = 0;  
            break;  
        }  
        else{  
            continue;  
        }  
    }  
}

void printQueue(int arr[]){  
    for (int i = 0; i < 5; i++){  
        printf("%d", arr[i]);  
    }  
    printf("\n");  
}

int main(void){  
    int arr[5] = {0,};  
    printf("원래 큐: ");  
    for (int i = 0; i < 5; i++){  
        arr[i] = i+1;  
        printf("%d", arr[i]);  
    }  
    printf("\n");

    printf("Deque 첫 번째 실행 후: ");
    Deque(arr);
    printQueue(arr);

    printf("Deque 두 번째 실행 후: ");
    Deque(arr);
    printQueue(arr);
}
```

</div></details>

코드 실행 시 결과는 다음과 같다.

> 원래 큐: 12345  
> Deque 첫 번째 실행 후: 02345  
> Deque 두 번째 실행 후: 00345

위 코드를 반복적으로 실행할 경우 모든 Queue의 원소가 0이 되고 배열의 저장 공간에도 한계가 발생하므로,

추가적인 Enqueue를 실행할 수 없는 문제점이 발생한다.

이런 단점을 해결하기 위해 순환 큐라는 구조를 사용할 수 있다.

* * *

### 4\. 순환 큐의 특징과 구현

&nbsp;순환 큐는 이런 기존 큐의 문제점을 해결하기 위해 시작(Front)과 끝(Back)을 연결해서 효율적인 삽입-삭제 연산이 가능하도록 고안된 큐를 말한다.  
<img src="https://i.namu.wiki/i/KOfKNZ2LdH6PDNwnpCeLckVCYS_4_RqXoTqBO2AJt72sjDCovEv4L_hkOC1ZQYenvGquOxQgV71sIoFA967HUw.webp" class="jop-noMdConv" width="706" height="368">

위의 그림처럼 Dequeue를 실행하면, 다음 원소가 front로 지정되고, Enqueue를 실행하면 새로 들어온 원소가 rear 혹은 back으로 지정된다.

단, 순환 큐를 구현할 때에는 큐의 공백 상태와 포화 상태를 고려해야 한다.

두 케이스 모두 Front와 Rear가 만나는 경우이기 때문에(rear가 배열의 끝을 넘어 다시 0으로 돌아옴), 두 상태를 구분하기 위해서 실제 필요한 용량보다 1 크게 만들어야 한다.

이렇게 하면 공백 상태와 포화 상태 발생 시, 공백 상태에는 Front와 Rear가 같은 지점을, 포화 상태에는 Rear가 Front보다 1 작은 값을 가지므로

상태 구분이 수월해진다.

이러한 순환 큐를 구현하기 위해서는 '큐 생성/소멸', '노드 삽입', '노드 제거', '공백 상태 확인', '포화 상태 확인' 등의 연산이 필요하다.

자세한 순환 큐 구현 과정은 아래와 같다.

* * *

### 1\. 노드 & 순환 큐 선언

```c
// 순환 큐 노드 선언
typedef struct tagNode{
    int data;
} Node;

// 순환 큐 선언
typedef struct tagCircularQueue{
    int capacity; // Queue의 용량
    int Front; // Front의 위치
    int Rear; // Rear의 위치
    Node* Nodes; //노드 배열 선언
} CircularQueue;
```

노드의 구조는 링크드 리스트를 구현할 때와 비슷하다.

그리고 큐를 선언할 때, Queue의 용량을 저장할 capacity, Front의 위치를 저장할 변수, Rear의 위치를 저장할 변수를 선언하고,

배열 형태로 큐를 구현할 것이기 때문에 Nodes를 통해 노드 배열을 선언한다.

* * *

### 2\. 순환 큐 생성 & 소멸

```c
// 순환 큐 생성
void createQueue(CircularQueue** Queue, int capacity){
    // 큐를 자유 저장소에 생성
    (*Queue) = (CircularQueue*)malloc(sizeof(CircularQueue));
    
    // 입력된 capacity + 1 만큼의 노드를 자유 저장소에 생성: 큐가 다 차게 되면 Rear가 0으로 초기화되기 때문
    (*Queue)->Nodes = (Node*)malloc(sizeof(Node) * (capacity + 1));
    
    (*Queue)->capacity = capacity;
    (*Queue)->Front = 0;
    (*Queue)->Rear = 0;
}
```

순환 큐를 생성하는 createQueue에서는 Queue를 이중 포인터로 선언해서 Nodes를 가리킬 수 있게 하고,

크기에 맞게 각각 malloc를 통해 용량을 할당한다.

주의해야 할 점은 capacity의 크기보다 1 크게 배열 사이즈를 만들어서 큐가 비어있을 경우와 꽉찬 경우를 구분할 수 있게 하는 것이다.

```c
// 순환 큐 소멸
void destroyQueue(CircularQueue* Queue){
    free(Queue->Nodes);
    free(Queue);
}
```

순환 큐의 소멸은 Nodes 배열을 먼저 메모리 해제하고 Queue의 메모리를 해제하는 식으로 실행한다.

* * *

### 3\. 노드 삽입 연산(Enqueue)

```c
// 노드 삽입 연산
void Enqueue(CircularQueue* Queue, int data){
    int position = 0;
    
    if (Queue->Rear == Queue->capacity){ //Rear가 capacity와 동일하면: Rear가 배열 끝까지 갔으면
        position=Queue->Rear;
        Queue->Rear = 0;
    }
    else{
        position=Queue->Rear++;
    }
    Queue->Nodes[position].data = data; //Rear의 다음 순서에 노드 넣기
}
```

노드 삽입을 수행하는 Enqueue에서는 position변수를 통해 해당 위치에 data를 넣을 수 있게 한다.

Rear와 capacity가 동일하다는 것은 Queue에서의 Enqueue연산을 통해 Rear의 위치가 점점 뒤로 향하면서 배열의 끝까지 도달했다는 것을 의미한다.

그렇기 때문에 position 변수에 Rear의 위치 값을 할당하고, 다시 Rear의 값은 0으로 초기화한다.

만약 Rear와 capacity가 동일하지 않다면, 후위 연산을 통해서 position에 Rear의 현재 위치 값을 할당하고 Rear의 값을 1 증가 한다(Rear++).

그리고 마지막으로 Queue가 가리키는 Nodes 배열의 position에 해당하는 위치에 data의 값을 넣어준다.

* * *

### 4\. 노드 제거 연산(Dequeue)

```c
// 노드 제거 연산
int Dequeue(CircularQueue* Queue){
    int position = Queue->Front;
    
    if (Queue->Front == Queue->capacity){ //Front가 capacity와 같으면 = Dequeue를 capacity-1만큼 수행했음!!
        Queue->Front = 0; //Front 0으로 초기화
    }
    else
        Queue->Front++;

    return Queue->Nodes[position].data;
}
```

노드 제거를 수행하는 Dequeue 연산에서는 노드 삽입과는 반대로 position 변수에 Front의 위치 값을 할당한다.

그리고 만약 Front가 Queue의 용량(capacity)과 동일하다면, Dequeue를 capacity-1만큼 수행해서 Front의 위치가 배열의 끝에 있는 것이기 때문에,

Front의 값을 다시 0으로 초기화한다.

만약 Front와 capacity가 다르다면, Front의 값을 1 증가한다.

그리고 position에 해당하는 위치의 data 값을 반환한다.

* * *

### 5\. 공백 상태 확인(IsEmpty)

```c
// 공백 상태 확인
int IsEmpty(CircularQueue* Queue){
    return (Queue->Front == Queue->Rear);
}
```

Queue가 공백 상태인지 확인하기 위해서는 Front와 Rear의 값이 동일한지 따지면 된다.

Front와 Rear의 값이 동일한 경우는 두 값이 모두 0일 때이기 때문이다.

* * *

### 6\. 포화 상태 확인(isFull)

```c
// 포화 상태 확인
int IsFull(CircularQueue* Queue){
    if (Queue->Front < Queue->Rear){
        return (Queue->Rear - Queue->Front) == Queue->capacity;
    }
    else{
        return (Queue->Rear+1) == Queue->Front;
    }
}
```

Queue의 포화 상태를 확인하기 위해서는 Front의 위치가 Rear보다 앞에 있는지, 아니면 그 반대로 Front가 Rear의 뒤에 있는지(혹은 동일한지) 확인하면 된다.

만약 Front가 Rear보다 앞에 있다면,  Rear-Front의 값이 Queue의 용량과 동일한지 따지면 된다.

예를 들어 전체 용량이 3인 큐에서, Front가 0이고, Rear가 2일 경우 이 둘의 차이인 2가 3과 동일한지 따지면 된다.

Rear가 2라는 것은 노드 삽입(Enqueue)연산 후에 후위 연산을 통해서 증가한 Rear의 값이 2라는 것으로,

아직 Rear가 더미 노드에 도달하지 않았으므로  Enqueue 연산을 한 번 더 수행할 수 있는 것을 의미한다.

만약 Rear가 3인 경우라면, 후위 연산을 통해 증가한 Rear의 값이 3이고, 이는 Rear의 위치가 더미 노드에 도달한 것을 의미하며,

더 이상 Enqueue 연산을 수행할 수 없음을 의미한다.

반대로 Front가 Rear보다 뒤에 있거나 위치가 같다면, Rear+1과 Front의 값이 동일한지 따지면 되는데,

만약 똑같이 전체 용량이 3인 큐에서 Rear가 0이고, Front가 2라면, `(0+1) != 2`이므로 0이 반환된다.

자세히 생각해보면 세 칸 중에서 0번째 칸에 Rear가 있고, 마지막 칸에 Front가 있으므로 그 가운데인 칸에 다시 Enqueue연산을 수행할 수 있다는 것을 알 수 있다.

만약 Rear와 Front가 둘 다 0이더라도, `(0+1) != 0`이 되며, 결국 이 경우에도 추가적인 Enqueue연산을 수행할 수 있음을 보여준다.

* * *

### 7\. 큐 사이즈 확인(GetSize)

```c
// 큐 사이즈 확인
int GetSize(CircularQueue* Queue){
    if(Queue->Front <= Queue->Rear){
        return Queue->Rear - Queue->Front;
    }
    else{
        return Queue->Rear + (Queue->capacity - Queue->Front) + 1;
    }
}
```

큐 사이즈를 확인하기 위해선 먼저 Front가 Rear보다 앞에 있거나 같은 위치에 있는지 따져본다.

Front가 Rear보다 앞에 있거나 같은 위치에 있다면 Rear에서 Front를 뺀 값을 반환한다.

예를 들어 Front가 0이고 Rear가 2라면 Enqueue 연산을 실행하고 1 증가한 Rear의 크기가 2라는 것이며,

결국 큐의 원소는 0번째와 1번째 인덱스에 들어있으므로 크기는 2가 된다.

반대로 Front가 Rear보다 뒤에 있는 경우에는 Rear의 값에 (capacity-Front)를 더하고 여기에 1을 더한다.

Front가 2이고 Rear가 0인 경우에는(큐 전체 용량은 3이라 가정),

&nbsp;`0+(3-2)+1`인 2가 반환 되는데, 큐의 전체 크기가 3이면 `[ ] [ ] [ ]` 같은 공간에서 `[Rear] [ ] [Front]`의 형태로 큐에 원소가 차 있는 것을 의미한다.

결국 해당 큐에는 2개의 원소가 있으므로 2가 반환 된다.

* * *

### 전체 구현 코드

```c
#include <stdio.h>
#include <stdlib.h>

// 순환 큐 노드 선언
typedef struct tagNode{
    int data;
} Node;

// 순환 큐 선언
typedef struct tagCircularQueue{
    int capacity; // Queue의 용량
    int Front; // Front의 위치
    int Rear; // Rear의 위치
    Node* Nodes; //노드 배열 선언
} CircularQueue;

// 순환 큐 생성
void createQueue(CircularQueue** Queue, int capacity){
    // 큐를 자유 저장소에 생성
    (*Queue) = (CircularQueue*)malloc(sizeof(CircularQueue));
    
    // 입력된 capacity + 1 만큼의 노드를 자유 저장소에 생성: 큐가 다 차게 되면 Rear가 0으로 초기화되기 때문
    (*Queue)->Nodes = (Node*)malloc(sizeof(Node) * (capacity + 1));
    
    (*Queue)->capacity = capacity;
    (*Queue)->Front = 0;
    (*Queue)->Rear = 0;
}

// 순환 큐 소멸
void destroyQueue(CircularQueue* Queue){
    free(Queue->Nodes);
    free(Queue);
}

// 노드 삽입 연산
void Enqueue(CircularQueue* Queue, int data){
    int position = 0;
    
    if (Queue->Rear == Queue->capacity){ //Rear가 capacity와 동일하면: Rear가 배열 끝까지 갔으면
        position=Queue->Rear;
        Queue->Rear = 0;
    }
    else{
        position=Queue->Rear++;
    }
    Queue->Nodes[position].data = data; //Rear의 다음 순서에 노드 넣기
}

// 노드 제거 연산
int Dequeue(CircularQueue* Queue){
    int position = Queue->Front;
    
    if (Queue->Front == Queue->capacity){ //Front가 capacity와 같으면 = Dequeue를 capacity만큼 수행했음!!
        Queue->Front = 0; //Front 0으로 초기화
    }
    else
        Queue->Front++;

    return Queue->Nodes[position].data;
}

// 공백 상태 확인
int IsEmpty(CircularQueue* Queue){
    return (Queue->Front == Queue->Rear);
}

// 포화 상태 확인
int IsFull(CircularQueue* Queue){
    if (Queue->Front < Queue->Rear){
        return (Queue->Rear - Queue->Front) == Queue->capacity;
    }
    else{
        return (Queue->Rear+1) == Queue->Front;
    }
}

// 큐 사이즈 확인
int GetSize(CircularQueue* Queue){
    if(Queue->Front <= Queue->Rear){
        return Queue->Rear - Queue->Front;
    }
    else{
        return Queue->Rear + (Queue->capacity - Queue->Front) + 1;
    }
}


int main(void){
    
    CircularQueue* Queue = NULL;
    createQueue(&Queue, 5);
    
    // 큐에 원소 삽입
    for (int i = 0; i < 5; i++){
        Enqueue(Queue, i+1);
    }
    
    printf("원래 큐:");
    for (int i = 0; i < 5; i++){
        printf("%d", Queue->Nodes[i].data);
    }
    printf("\n");
    
    printf("큐 사이즈:");
    printf("%d\n", GetSize(Queue));
    
    printf("큐 비어있는지 확인: %d\n", IsEmpty(Queue)); //0이면 안비어있음, 1이면 비어있음
    printf("큐 가득 차있는지 확인: %d\n", IsFull(Queue)); //0이면 안 가득, 1이면 가득있음
    
    for (int i = 0; i < 5; i++){
        Dequeue(Queue);
        printf("큐 사이즈: %d\n", GetSize(Queue));
    }
    
    printf("큐 비어있는지 확인: %d\n", IsEmpty(Queue)); //0이면 안비어있음, 1이면 비어있음
    printf("큐 가득 차있는지 확인: %d\n", IsFull(Queue)); //0이면 안 가득, 1이면 가득있음
    
}
```

**출력결과**

> 원래 큐:12345  
> 큐 사이즈:5  
> 큐 비어있는지 확인: 0  
> 큐 가득 차있는지 확인: 1  
> 큐 사이즈: 4  
> 큐 사이즈: 3  
> 큐 사이즈: 2  
> 큐 사이즈: 1  
> 큐 사이즈: 0  
> 큐 비어있는지 확인: 1  
> 큐 가득 차있는지 확인: 0