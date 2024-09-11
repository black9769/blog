---
title: "[OS] IPC(Inter Process Comunication)"
published: 2024-06-24
description: ''
image: ''
tags: [CS, OS, Process]
category: 'Operating System'
draft: false 
---
# IPC(Inter Process Comunication)

## 개요
[Muti Process](/blog/posts/operatingsystem/os-muti-processing-vs-muti-thread/)에서 Muti Process 환경을 구성하기 위해
IPC를 활용해야 한다고 했다. IPC는 프로세스간 통신을 위해서 만들어졌다. 

### 왜일까?
Thread와 비교를 적절한지 긴가민가 하지만, 일단 Thread와 비교를 하자면 Muti Thread환경에서는 Stack영역을 제외하고 공유하고 있기때문에
따로 통신 체계가 필요없이 전역변수를 통해서 동작하게 된다. 하지만, Process는 독립적으로 운영되기 때문에 Muti Process환경을 위해서는 통신이
필요한다는 것이다.  
또한, 데이터 공유, 동기화, 자원관리가 필요하기 때문이다.

### 어디서?
OS에서 제일 안전한 지대는 Kernal 영역이다. Kernal영역은 아무도 들어올 수도 없고 그 영역자체가 격리되어있어 보안에 매우
유리하다.

---

## IPC 종류

### Pipe(파이프)
- 특징
  - 하나의 Process는 Write, 다른 프로세스는 Read만 한다.
  - 부모자식간의 통신이 이루어진다.
  - 프로세스간 양방향 통신을 하지만 한쪽에서 수신중이면 다른쪽은 받기만해야한다. ➡️ 반이중 통신

