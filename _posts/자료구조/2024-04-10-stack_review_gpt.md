---
title : "스택 자료구조와 관련된 지피티의 리뷰(백준 9012: 괄호)"
date : 2024-04-10 20:38:30 +/-TTTT
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

이 글은 백준 알고리즘 문제 9012번 문제와 관련해서

Chat-GPT에게 코드 리뷰를 부탁한 결과에 대해 작성한 글입니다.

* * *

오늘 백준을 풀다가

스택 관련 문제에서 오류를 찾기가 어려워 지피티에게 물어보았다.

문제는 다음과 같다.

[https://www.acmicpc.net/problem/9012](https://www.acmicpc.net/problem/9012 "백준 9012번:괄호")[](https://www.acmicpc.net/problem/9012)

### 내가 작성한 코드

```c++
#include <iostream>
#include <string.h>
#include <stack>
using namespace std;

int main() {
    int num;
    string input;
    cin >> num;

    stack<int>stack;

    for (int i = 0; i < num; i++) {
        cin >> input;
        char plus = '(';
        char minus = ')';
        int total = 0;
        for (int j = 0; j < input.length(); j++) {
            if (input[j] == plus) {
                stack.push(1);
                total += 1;
                //total += 1;
            }
            else if (input[j] == minus) {
                if (stack.empty() == 0) {
                    total -= 1;
                    stack.pop();
                }
                else {
                    total -= 1;
                    break;
                }
            }
            else
                break;
        }
        input = "";
        if (total == 0)
            cout << "YES" << endl;
        else
            cout << "NO" << endl;
    }

    return 0;
}
```

지피티는 내가 작성한 코드에 대해서 아래의 개선점을 말해주었다.

> 주어진 코드를 살펴보니 스택을 활용하여 괄호 문자열을 검사하는 방법으로 접근하셨습니다. 그러나 몇 가지 수정이 필요합니다. 아래는 코드의 수정 사항입니다.
> 
> 1.  스택을 `int`형으로 선언했는데, 이는 괄호의 여부를 확인하는 데 사용할 수 없습니다. 스택에는 괄호의 쌍을 맞추기 위해 괄호의 종류를 저장해야 합니다. 따라서 스택을 `char`형으로 선언해야 합니다.
>     
> 2.  스택을 `char`형으로 수정하면서, 괄호 문자열의 여닫는 괄호를 `char` 상수로 선언하여 코드를 더 명확하게 만들 수 있습니다.
>     
> 3.  스택이 비어있는지 확인할 때 `stack.empty()`를 사용하는 것은 올바른 방법입니다. 그러나 `stack.empty() == 0`으로 확인하는 것은 C++에서 표준적인 방법은 아닙니다. 대신에 `!stack.empty()`로 확인하는 것이 좋습니다.
>     
> 4.  입력된 문자열이 올바른 괄호 문자열인지를 판단할 때에도 모든 문자가 '(' 또는 ')'인지 확인해야 합니다. 그렇지 않으면 다른 문자가 섞여 있는 경우에도 올바른 결과를 얻을 수 없습니다.
>     

* * *

### 지피티가 작성한 코드

```c++
#include <iostream>
#include <stack>
#include <string>

using namespace std;

int main() {
    int num;
    cin >> num;

    for (int i = 0; i < num; i++) {
        string input;
        cin >> input;

        stack<char> stack;
        bool is_valid = true;

        for (int j = 0; j < input.length(); j++) {
            char current_char = input[j];
            if (current_char == '(') {
                stack.push(current_char);
            } else if (current_char == ')') {
                if (!stack.empty() && stack.top() == '(') {
                    stack.pop();
                } else {
                    is_valid = false;
                    break;
                }
            } else {
                is_valid = false; // 괄호가 아닌 문자가 있음
                break;
            }
        }

        if (is_valid && stack.empty()) {
            cout << "YES" << endl;
        } else {
            cout << "NO" << endl;
        }
    }

    return 0;
}
```

> 1.  `#include` 문장:
>     
>     - `<iostream>`: 입력 및 출력을 위한 표준 라이브러리.
>     - `<stack>`: 스택 자료구조를 사용하기 위한 표준 라이브러리.
>     - `<string>`: 문자열을 다루기 위한 표준 라이브러리.
>     - 위 세 개의 헤더 파일을 포함시킵니다.
> 2.  `using namespace std;`:
>     
>     - 표준 라이브러리의 `std` 네임스페이스를 사용합니다. 이 문장을 통해 `cin`, `cout`, `stack`, `string` 등의 이름을 직접 사용할 수 있습니다.
> 3.  `int main() { ... }`:
>     
>     - 프로그램의 진입점입니다. 여기서부터 프로그램이 실행됩니다.
> 4.  `int num; cin >> num;`:
>     
>     - 테스트 케이스의 개수를 입력으로 받습니다.
> 5.  `for (int i = 0; i < num; i++) { ... }`:
>     
>     - 테스트 케이스의 개수만큼 반복합니다.
> 6.  `string input; cin >> input;`:
>     
>     - 괄호 문자열을 입력으로 받습니다.
> 7.  `stack<char> stack;`:
>     
>     - 괄호의 짝을 맞추기 위해 사용할 스택을 선언합니다.
> 8.  `bool is_valid = true;`:
>     
>     - 현재 괄호 문자열이 올바른 괄호 문자열인지 여부를 저장하는 변수를 선언하고 초기값을 `true`로 설정합니다.
> 9.  `for (int j = 0; j < input.length(); j++) { ... }`:
>     
>     - 입력된 문자열을 순회하면서 각 문자를 확인합니다.
> 10. `char current_char = input[j];`:
>     
>     - 현재 순회 중인 문자를 저장합니다.
> 11. `if (current_char == '(') { ... } else if (current_char == ')') { ... } else { ... }`:
>     
>     - 현재 문자가 여는 괄호인지, 닫는 괄호인지를 확인합니다. 다른 문자인 경우도 처리합니다.
> 12. `if (is_valid && stack.empty()) { ... } else { ... }`:
>     
>     - 모든 문자를 확인한 후, 스택이 비어있고, 괄호 문자열이 올바른 경우 "YES"를 출력하고, 그렇지 않은 경우 "NO"를 출력합니다.
> 13. `return 0;`:
>     
>     - 프로그램의 정상 종료를 나타냅니다.
> 
> 이렇게 코드는 입력으로 받은 각각의 문자열에 대해 괄호의 짝을 맞추어 올바른 괄호 문자열인지를 판별하고, 결과를 출력합니다.

챗 지피티가 작성한 코드를 바탕으로 코드를 작성해 제출을 해봤는데 "정답"처리가 되었다.

&nbsp;

### 지피티의 코드리뷰를 통해서 배운 점

&nbsp;

1.  스택이 비어있는지 확인할 때 `stack.empty()`를 사용하는 것은 올바른 방법이지만, `stack.empty() == 0`으로 확인하는 것은 C++에서 표준적인 방법이 아니라는 것. 대신에 `!stack.empty()`로 확인하는 것이 좋다는 것을 알게 되었다. 예전에 'stack.empty()' 함수가 스택이 비어있을 때 1을 리턴하는 것을 알게 되어서 이렇게 코드를 작성했지만, 스택이 비어있는지 아닌지를 좀 더 확실하게 알기 위해서는 !연산자를 통해 판별하는 게 좋겠다는 생각을 하게 되었다.
2.  그리고 스택 자료구조의 자료형을 정수형으로 선언을 했지만, 문제 특성상 입력된 문자를 구분하고 이를 바탕으로 결과를 출력해야 하기 때문에 문자 자료형(char)의 스택 구조를 선언하는게 더 좋겠다는 생각을 하였다.
3.  마지막으로 VPS문자를 판별하라는 문제의 요구 특성 상, 스택구조가 비어있지 않으면서 스택의 맨 위(top)의 값이 ")"이라는 조건을 만족할 때 pop연산을 수행하게끔 선언하는게 VPS문자를 판별하는 좀 더 확실한 조건이 될 것 같다는 생각을 했다.
4.  그리고 결국엔 VPS 문자인지 판단해야 하므로 숫자 값의 범위 조건으로 판단을 하기 보다는 true 또는 false값을 담을 수 있는 bool 자료형을 통해서 판단 조건을 할당하는게 더 정확할 것 같다는 생각을 하게 되었다. 

&nbsp;

&nbsp;

&nbsp;