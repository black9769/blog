---
title: '[BOJ] BOJ템플릿'
published: 2024-06-16
description: ''
image: ''
tags: [BOJ, ]
category: 'Problem Solving'
draft: true 
---


# BOJ(문제 번호)
![Alt text](./BOJ/BOJICON.png)

## 소개

## 문제

## 코드🖥️
```cpp
#include <iostream>

using namespace std;

int k, n;

int money[11] = { 0, };

int main() {
	cin >> k >> n;
		
	for (int i = k - 1; i >= 0; i--) {
		cin >> money[i];
	}

	int cnt = 0;

	for (int i = 0; i < k; i++) {
		if (n >= money[i]) {
			cnt += n / money[i];
			n %= money[i];
		}
	}

	cout << cnt;

	return 0;
}
```

---

## 해설

## 느낀점


