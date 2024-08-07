---
title: "[ALGO] Pseudo 작성법"
published: 2024-08-08
description: 'Pseudo 작성방법에 대해 알아본다.'
image: ''
tags: ["Algorithm", "Concept"]
category: 'Algorithm'
draft: false 
---
# 개요

`Pseudo`라는 말은 파파고의 사전에 따르면

> 📖 사전적의미
>
> 허위의, 가짜의; 모조의  
> 꾸며 보이는 사람, 사칭자

위와 같은 뜻을 갖고 있다. (사이비라는 뜻도 포함한다.)  

뜻만 보면 나쁜 뜻으로 해석되지만, 개발자들에게 Pseudo코드를 작성하는 것은 처음 코딩을 하는 사람에게 필요한 작업중 하나이다.

코딩테스트를 볼때, Pseudo코드는 필수적인 존재라고 볼수 있다. `빡구현을 위한 코딩테스트`를 풀때도 중요하다.

# 작성방법

작성방법은 영어로써도 되고 한국어로 써도 된다. 나의 코드가 어떻게 흘러가는지 아는것이 가장 큰 맹점이고 어떤 변수와 함수들을 선언하여 코드의 중복을 삭제할 수 있다.  

실제코드를 작성하는 것처럼 코드를 짜면 된다. 논리적인 구조에 맞춰서 호출과 선언도 마찬가지로 진행하면 된다.

## 예시

Pseudo 코드를 작성하는 예시를 `대표 정렬 알고리즘`, `탐색 알고리즘` , `배열 뒤집기`를 통해서 알아보겠다.

### [버블 정렬](/blog/posts/algorithm/algo-sort/#1-버블정렬)

```plaintext
버블 정렬(배열):
  배열의 길이 n
  반복 i = 0부터 n-1까지:
    반복 j = 0부터 n-i-2까지:
      만약 배열[j] > 배열[j+1]:
        배열[j]와 배열[j+1]을 교환
```

### [선택 정렬](/blog/posts/algorithm/algo-sort/#2-선택정렬)

```plaintext
선택 정렬(배열):
  배열의 길이 n
  반복 i = 0부터 n-1까지:
    최소값 인덱스 = i
    반복 j = i+1부터 n-1까지:
      만약 배열[j] < 배열[최소값 인덱스]:
        최소값 인덱스 = j
    배열[i]와 배열[최소값 인덱스]를 교환
```

### [삽입 정렬](/blog/posts/algorithm/algo-sort/#3-삽입정렬)

```plaintext
삽입 정렬(배열):
  배열의 길이 n
  반복 i = 1부터 n-1까지:
    현재값 = 배열[i]
    j = i - 1
    반복 (j >= 0 그리고 배열[j] > 현재값):
      배열[j+1] = 배열[j]
      j = j - 1
    배열[j+1] = 현재값
```

### [병합 정렬](/blog/posts/algorithm/algo-sort/#4-병합정렬)

```plaintext
병합 정렬(배열):
  만약 배열의 길이 <= 1:
    반환 배열
  중간 인덱스 = 배열 길이 // 2
  왼쪽 절반 = 병합 정렬(배열[0:중간])
  오른쪽 절반 = 병합 정렬(배열[중간:])
  반환 병합(왼쪽 절반, 오른쪽 절반)

병합(왼쪽, 오른쪽):
  결과 = 빈 배열
  왼쪽 인덱스 = 0
  오른쪽 인덱스 = 0
  반복 (왼쪽 인덱스 < 왼쪽 길이 그리고 오른쪽 인덱스 < 오른쪽 길이):
    만약 왼쪽[왼쪽 인덱스] <= 오른쪽[오른쪽 인덱스]:
      결과에 왼쪽[왼쪽 인덱스] 추가
      왼쪽 인덱스 += 1
    아니면:
      결과에 오른쪽[오른쪽 인덱스] 추가
      오른쪽 인덱스 += 1
  결과에 남은 왼쪽 요소들 추가
  결과에 남은 오른쪽 요소들 추가
  반환 결과
```

### [퀵 정렬](/blog/posts/algorithm/algo-sort/#5-퀵정렬)

```plaintext
퀵 정렬(배열):
  만약 배열의 길이 <= 1:
    반환 배열
  피벗 = 배열[배열의 길이 // 2]
  왼쪽 = [x for x in 배열 if x < 피벗]
  중간 = [x for x in 배열 if x == 피벗]
  오른쪽 = [x for x in 배열 if x > 피벗]
  반환 퀵 정렬(왼쪽) + 중간 + 퀵 정렬(오른쪽)
```

### [힙 정렬](/blog/posts/algorithm/algo-sort/#6-힙정렬)

```plaintext
힙 정렬(배열):
  배열의 길이 n
  반복 i = n//2 - 1부터 0까지:
    힙 생성(배열, n, i)
  반복 i = n-1부터 1까지:
    배열[0]와 배열[i]를 교환
    힙 생성(배열, i, 0)

힙 생성(배열, n, i):
  가장 큰 값 = i
  왼쪽 자식 = 2*i + 1
  오른쪽 자식 = 2*i + 2
  만약 왼쪽 자식 < n 그리고 배열[왼쪽 자식] > 배열[가장 큰 값]:
    가장 큰 값 = 왼쪽 자식
  만약 오른쪽 자식 < n 그리고 배열[오른쪽 자식] > 배열[가장 큰 값]:
    가장 큰 값 = 오른쪽 자식
  만약 가장 큰 값 != i:
    배열[i]와 배열[가장 큰 값]을 교환
    힙 생성(배열, n, 가장 큰 값)
```

### [선형탐색](/blog/posts/algorithm/algo-searching/#선형탐색)

```plaintext
선형 탐색(배열, 값):
  배열의 각 요소에 대해 반복:
    만약 요소 == 값:
      반환 요소의 인덱스
  반환 -1  # 값을 찾지 못한 경우
```

### [이진탐색](/blog/posts/algorithm/algo-searching/#이진탐색)

```plaintext
이진 탐색(배열, 값):
  왼쪽 = 0
  오른쪽 = 배열의 길이 - 1
  반복 (왼쪽 <= 오른쪽):
    중간 = (왼쪽 + 오른쪽) // 2
    만약 배열[중간] == 값:
      반환 중간
    만약 배열[중간] < 값:
      왼쪽 = 중간 + 1
    아니면:
      오른쪽 = 중간 - 1
  반환 -1  # 값을 찾지 못한 경우
```

### [그래프탐색](/blog/posts/algorithm/algo-searching/#그래프-탐색)

#### [BFS](/blog/posts/algorithm/algo-searching/#bfs너비우선탐색)

```plaintext
BFS(그래프, 시작 정점):
  방문 = 빈 집합
  큐 = [시작 정점]
  반복 큐가 빌 때까지:
    정점 = 큐에서 제거
    만약 정점이 방문에 없다면:
      방문에 정점 추가
      정점의 모든 인접 정점을 큐에 추가
  반환 방문
```

#### [DFS](/blog/posts/algorithm/algo-searching/#dfs깊이우선탐색)

```plaintext
DFS(그래프, 시작 정점):
  방문 = 빈 집합
  스택 = [시작 정점]
  반복 스택이 빌 때까지:
    정점 = 스택에서 제거
    만약 정점이 방문에 없다면:
      방문에 정점 추가
      정점의 모든 인접 정점을 스택에 추가
  반환 방문
```

### 배열뒤집기

```plaintext
배열 뒤집기(배열):
  왼쪽 = 0
  오른쪽 = 배열의 길이 - 1
  반복 (왼쪽 < 오른쪽):
    배열[왼쪽]과 배열[오른쪽]을 교환
    왼쪽 += 1
    오른쪽 -= 1
```
