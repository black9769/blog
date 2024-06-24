---
title: "[OS] Muti Processing VS. Muti Thread"
published: 2024-06-24
description: ''
image: ''
tags: [CS, OS, Process]
category: 'Operating System'
draft: false 
---

# Muti? Process? Thread?
## 개요

### 복습
[Process](/blog/posts/operatingsystem/os-processs/)도 알아보고 [Thread](/blog/posts/operatingsystem/osthread/)에 대해서도 알아보았다.
Muti Thread는 잠깐 은행과 은행 창구를 비교하면서 이야기하기도 했다.  

OS는 항상 선행에 대한 공부를 할 필요가 있기때문에 모르겠다면 다시 찾아보고 알고 지금 Muti 환경에 대해서 살펴보자.

Process의 Trasition을 보면서 Ready → Running, Running → Ready, Running → Waiting 갈때 Context에 대해서 알아보았다. Context가 Switch되면서 Overhead가 발생한다는
사실도 알았다. 즉, 다시 설명하면 Context Swithcing은 Process가 가진 Meta Data들을 교환하는 것이다. 

### 왜? Context Switching이 일어나지?
CPU는 1 Task만 할 수 있다. 그런데 Interrupt나 갑자기 중요한 이벤트가 발생한다면 기존 작업을 중단하고 다른 중요한 작업에게 넘겨줘야한다. 그 작업이 끝나면
또 원래 작업이 정상적으로 동작 할 수 있도록 해야한다. 이것도 CPU 스케듈링과도 매우 연관이 있다.  

> 💡 고민해보자!  
> 선점형이랑 비선점형중에 Context Swithcing이 자주 일어나는 스케듈링은 무엇일까?

---

## Overhead
![Alt text](./ProcessAsset/img.gif)
이미 우리는 Context가 교환될때 데이터 셋이 교환되면서 Overhead가 발생하는 것을 알고 있다.

### Overhead가 크다는건 어떤 의미일까?
처음으로 코딩을 배우게 되면 교환(Swap) 알고리즘을 배운다. A라는 컵엔 오렌지맛 환타가, B라는 컵에 오물이 담겨 있다.
나는 오물이 담겨있는 B컵 디자인이 너무 예뻐서 오렌지 환타를 넣어먹고 싶다.  

그러면 어떻게 할것인가? 오물을 바닥에 버리면 환경오염된다.
냅다 그냥 B에 오물이 담겨 있는데 오렌지맛 환타를 A에서 부을건가?  

세상에 그런 바보는 없을 거다. 다른 빈컵을 찾아서 오렌지맛 환타를 A에서 덜어두고 B를 A에 옮기고 B컵에 오렌지맛 환타를 덜겠다.
이게 Swap이다.

```
Procedure swap(a, b):
temp ← a
a ← b
b ← temp
```

내가 이렇게 구구절절 설명한건, **Overhead가 바로 빈컵**이라는 거다.
즉, 데이터 셋이 크다는것은 덜어낼 것을 위한 공간이 많이 필요하다는 것을 의미한다.

### 그렇다면 Thread가 클까? Process가 클까?
저번 포스팅을 읽고 학습을 해왔다면, 쉽게 알 수 있을 것이다. 당연히 자원을 더 많이 갖고 있는 **Process가 Overhead가 크고, Stack영역을 제외한
나머지 영역을 공유하는 Thread가 작을 것**이다.

---
### 그래서 Muti Process , Muti Thread가 뭔데?
도대체 Context Switchiing하고 Muti Process와 Thread가 무슨 상관관계 길래, 이번 포스팅의 결론이 지금 나오나 할 수도 있다.
사실 이 둘은 Process에서 배운 내용과 Thread에서 배운 내용의 최종 보스급의 내용이기 때문에 거의 다 알고 있어야한다.

### 두개를 왜 쓸까?
두개의 방식은 **병렬처리** 방식을 지원하기 때문이다. 하지만 두개 동작하는 방식은 아예 다르다.

### Muti Process
- 특징
  - 여러 프로세스를 동시에 실행하며 독립적은 주소공간을 갖고 있다.
  - 여러 프로세스를 처리하기 위해서는 프로세스간 내부 통신을 한다. 우리는 그것을 IPC라고 부른다.
- 장점
  - 하나의 프로세스가 비정상적은 동작을 해도 다른 프로세스에 영향을 주지 않는다.
- 단점
  - 프로세스가 가진 Context가 크기 때문에 Swithcing할때 Overhead크다.

### Muti Thread
- 특징
    - 각 스레드는 Stack영역을 제외한 나머지 영역을 공유한다.
- 장점
    - 메모리 사용이 효율적이다.
    - 주소 공간을 공유하기떄문에 스레드간 통신이 빠르다.
- 단점
    - 스레드간 동기화 문제가 발생한다. ➡️ Race Condition
    - 하나의 Thread가 Process 전체에 문제를 일으킬 수 있다.


| 특징       | Muti-Process             | Muti-Thread                     |
|----------|--------------------------|---------------------------------|
| 주소공간     | 각 프로세스는 독립적인 주소 공간을 가짐   | 동일한 주소 공간을 공유                   |
| 통신방식     | IPC(파이프, 메시지 큐 등)        | 메모리 공유, 간단한 변수 접근               |
| Overhead | 높음(메모리 매핑, 페이지 테이블 전환 등) | 낮음(주소 공간 공유로 인한 단순한 상태 저장 및 복원) |
| 안정성      | 	높음(프로세스 간 분리로 인한 보호)    | 낮음(스레드 실패 시 전체 프로세스에 영향)        |

---
## FINAL
ContextSwitching이나 Overhead는 다른 파트에서도 많이 나오는 분야이다. Thread를 알고 Process를 알았으니 서비스를 개발할때 항상 고민하고
상기 시키면서 개발 할 수 있는 기본 지식을 탄탄하게 만들 수 있다고 생각한다. 이제 누가 물어보면 제대로 답할 수 있겠지?