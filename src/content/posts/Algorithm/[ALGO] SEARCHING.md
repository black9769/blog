---
title: "[ALGO] Searching"
published: 2024-06-26
description: ''
image: ''
tags: ["Algorithm", "Searching", "Concept"]
category: 'Algorithm'
draft: false 
---
# 탐색 알고리즘
Sorting 알고리즘과 함께 같이 나오는 Searching 알고리즘이 있다. 내용은 그렇게 어렵지 않다. PS를 풀때도 항상 기본이 되는 알고리즘 기법인
정렬, 탐색 알고리즘을 기억하고 있자.

## 선형탐색
![Alt text](./SortingAsset/linear_search.gif)
선형 탐색은 배열에서 해당 값을 순차적으로 진행하면서 찾아내는것이다.
### 코드
```c++
int linearSearch(vector<int>& arr, int target) {
    for (size_t i = 0; i < arr.size(); ++i) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}
```
- 장점
  - 정렬되지 않아도 찾을 수 있다.
  - 무식한 방법이다.
- 시간복잡도 O(n)

## 이진탐색
![Alt text](./SortingAsset/binary-and-linear-search-animations.gif)
Binary Search라고 부르며, 배열에서 중간 값을 기준으로 탐색 범위를 반씩 줄여가며 목표값을 찾는다. **정렬이 필수로 이루어져있어야한다.**

### 코드
```c++
int binarySearch(const vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target)
            return mid;
        else if (arr[mid] < target)
            left = mid + 1;
        else
            right = mid - 1;
    }
    return -1;
}
```
- 장점
  - 정렬된 데이터에서 매우 효율적이다.
- 단점
  - 정렬과정이 이루어져야한다.
- 시간복잡도 O(log n)

## 그래프 탐색
### BFS(너비우선탐색)
![Alt text](./SortingAsset/Breadth-First-Search-Algorithm.gif)
비선형 자료구조 중 그래프를 활용한 탐색 기법이다. 선형 자료구조중 하나인 Queue를 구조를 활용하여 구현할 수 있다.
```c++
void BFS(vector<vector<int>>& adj, int start) {
    vector<bool> visited(adj.size(), false);
    queue<int> q;
    visited[start] = true;
    q.push(start);

    while (!q.empty()) {
        int v = q.front();
        q.pop();
        cout << v << " ";

        for (int i : adj[v]) {
            if (!visited[i]) {
                visited[i] = true;
                q.push(i);
            }
        }
    }
}
```
너비우선 탐색은 최단 거리 경로를 찾을때 가장 적합한 문제 풀이 방식이다. PS 문제에서 "최단거리"라는 키워드가 있으면 BFS로 푸는게 가장 나이스 한 
방법이다.

### DFS(깊이우선탐색)
![Alt text](./SortingAsset/Depth-First-Search.gif)
비선형 자료구조 중 그래프를 활용한 탐색 기법이다. 재귀적으로 구하거나 Stack 구조 방식을 활용하여 구현할 수 있다.

```c++
void DFSUtil(int v, vector<vector<int>>& adj, vector<bool>& visited) {
    visited[v] = true;
    cout << v << " ";
    for (int i : adj[v]) {
        if (!visited[i]) {
            DFSUtil(i, adj, visited);
        }
    }
}

void DFS(const std::vector<std::vector<int>>& adj, int start) {
    vector<bool> visited(adj.size(), false);
    DFSUtil(start, adj, visited);
}
```
해당 탐색 기법은 경로를 탐색할 때 활용된다. BFS는 최단거리 경로를 탐색하지만, DFS는 각 노드에서 가능한 멀리 있는 노드까지 방문한 후에,
더이상 갈 수 없으면 되돌아오면서 다른 경로를 탐색하는 방식이다. 예를 들면, 미로 찾기

---
# FINAL
선형 자료구조에서는 탐색을 할때 이미 정렬되어있는 구조가 가장 유리하다. 이진 탐색과 방법이 유사한 보간 기법을 이용한 보간 탐색도 있는데
해당 탐색기법도 이미 정렬이 되어있는 상태에서 활용해야한다.  

즉, 정렬과 탐색은 거의 하나와 같다. 우리가 자료를 검색할때도 정렬 후에 찾지 그냥 냅다 찾지는 않는다. 알고리즘의 핵심은 Sorting과 Searching이다.