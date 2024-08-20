---
title: "[ALGO] Stack & Queue"
published: 2024-07-04
description: ''
image: ''
tags: ["Algorithm", "Stack","Queue", "Concept"]
category: 'Algorithm'
draft: false 
---

# 개요

선형 자료구조의 대표로는 Stack, Queue가 대표적이다. 이를 알아보고 어떻게 구현하고, 다른 언어에서는 어떻게 선언되며 사용되는지 알아보자.

## Stack

Stack(스택)은 선형 자료구조의 대표이다.

이를 쉽게 설명하는 법은 컵을 생각하면된다. 우리가 컵에 다양한 색상의 모래를 넣는 쌓는 모습을 그려보자.

처음에는 흰 모래를 넣고, 두번째는 파란 모래, 세번째는 초록 모래를 쌓았다.

내가 흰 모래를 꺼내는 방법은 무엇일까?

반대로 가야한다. 먼저 초록 모래를 꺼내고, 파란 모래를 꺼내고, 흰 모래를 꺼내야한다.

즉, 후입선출이다. 가장 마지막에 들어간게 처음으로 나온다.

다른 예로 웹 브라우저의 뒤로 가기(goBack)도 마찬가지이다.

내가 이동한 url 경로들이 스택에 쌓여서 stack 넣고 뒤로가기 이벤트가 들어올때마다 하나씩 하나씩 빼내는 것을 의미한다.

## Stack 구현

Stack을 구현하기 위해서는 2가지 기능이 필요하다.

1. 초기화(init)
2. 스택에 넣기(push)
3. 스택에서 꺼내기(pop)
4. 최상위 데이터 보기(peek)
5. 빈 스택인지 확인(isEmpty)
6. 크기 확인(size)

### 초기화(init)

먼저 배열로 구현하기 위해서는 초기에 top value를 저장해야된다.
top value는 빈 스택에서는 -1이 된다.

그리고 하나씩 들어갈때마다 +1씩 증가하게 된다.

```plain text
    init(){
       arr = 고정된 크기의 배열 생성
       top = -1 //초기 위치 설정
    }
```

### 넣기(push)

스택에 넣는 것을 `push`라고 한다.
스택에 push하는 마지막 top value보다 +1 증가 되어 element가 들어가게된다.
하지만, 이미 정해둔 스택의 크기보다 크다면 element가 들어갈 자리는 없다.

```plain text
    push(element){
        if(스택이 가득 찼는지 확인) return 넣을 공간 없음
        top = top +1;
        arr[top] = element
    }
```

### 빼기(pop)

스택에서 빼는 것을 `pop`이라고 한다.
스택에 `pop`은 stack의 구조를 설명할때

```plain text
    pop(){
        if(top == -1) //스택이 비어있다면
            return 스택에 값이 없다.
        element = arr[top];
        top = top - 1;
        return element;
    }
```

### 최상단 값 보기(peek)

```plain text
    peek(){
        if(top == -1) //스택이 비어있다면
            return 스택에 값이 없다.
        return arr[top];
    }
```

### 빈 스택인지 확인(isEmpty)

```plain text
    isEmpty(){
        if(top == -1) //스택이 비어있다면
            return 스택에 값이 없다.
    }
```

### 스택 크기 확인(size)

```plain text
    size(){
        return top = top + 1;
    }
```

## 배열로 스택 구현하기 (C++)

```cpp
#include <iostream>

using namespace std;

class Stack{
private:
    int* data;
    int top;
    int max_size;

public:
    Stack(int size){
        max_size = size;
        data = new int[max_size];
        top = -1;
    }

    ~Stack(){//메모리 해제
        delete[] data;
    }

    
    void push(int element){
        if(top == max_size -1){
            cout << "스택이 가득 찼습니다" << "\n";
            return;
        }
        data[++top] = element;
    }

    void pop(){
        if(isEmpty()){
            cout << "스택이 이미 비었습니다." << "\n";
            return -1;
        }
        return data[top--];
    }

    void peek(){
      if(isEmpty()){
            cout << "스택이 이미 비었습니다." << "\n";
            return -1;
        }
        return data[top];      
    }

    bool is_empty(){
        return top == -1;
    }

    int size(){
        return top + 1;
    }
}

int main(){
    Stack s(5);

    s.push(10);
    s.push(20);
    s.push(4);
    s.pop();
    s.pop();
    s.push(10);

    return 0;
}
```

## C++에서 Stack 사용

```cpp
#include <stack>

stack<int> s;

//push
s.push(1);

//pop
s.pop();

//peek
s.top();

//size
s.size();

//empty
s.empty();
```

## Queue(큐)

Queue(큐)는 사전적 의미로는 `대기열`을 의미한다. 대기열은 무슨 의미인가?

맛집에 가기위해서 우리는 대기 줄을 선다. 그게 대기열이다. 내 앞팀이 순차적으로 다 들어가고 나서야
우리팀이 맛집에 들어가서 식사를 즐길 수 있다.

즉 선입선출을 의미한다. 먼저 들어온 팀이 먼저 나가는 것이다.

## Queue 구현

Queue는 선형자료구조로 선입선출이라는 것을 기억해야한다.

선입, 먼저 들어온 데이터가, 선출, 먼저 나간다.

1. 초기화
2. 넣기
3. 꺼내기
4. 최상단 값
5. 빈 큐인지 확인
6. 크기

이를 Pseudo 코드로 작성할 수 있다.

### 초기화
