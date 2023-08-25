---
layout: post
title: ' [Algorithm]Union-Find'
author: [KKH]
tags: ['Algorithm', '알고리즘']
image: img/Algorithm.png
date: '2023-08-25T23:00:00.000Z'
draft: false
---
# Union-Find

## Union-Find란?

union find알고리즘은 Disjoint-Set을 구현할 때 활용되는 알고리즘으로 **트리** 구조를 이용한다.  

집합이 트리형태로 표현되기 때문에 루트원소가 같으면 똑같은 집합에 속해있다고 볼 수 있다.  
루트노드가 해당 집합의 대표이름으로 불린다고 생각하면 쉬울 것 같다.

대표적으로 최소 신장 트리(MST)를 구현하는 **크루스칼 알고리즘**에 활용된다.

> Disjoint-set이란 A와 B집합이 있을 때 공유되는 원소가 없는 경우를 말한다. 상호 배타적 관계의 집합을 말한다.


## Method
1. Union(x,y)
	- x와 y가 속해 있는 집합을 합친다.
2. Find(x)
	- x가 속해있는 집합의 **루트**를 찾는다.(각 집합(set)은 트리 구조로 되어 있기 때문에 루트 원소를 가진다.)

## Implements
1. **Initialize** 
	1. 모든 원소의 개수가 N개로 주어져 있을 때 각 원소의 부모 원소를 저장하는 **parent배열**을 선언한다.
		=> int parent\[N]
	2. 모든 원소가 자기자신을 부모로 설정하도록 초기화 한다.
		=> parent\[n] = n ( 0 <= n < N)
		
2. **union(x,y)**
	1. Find함수를 통해 x와 y의 루트노드를 찾고 다르면(다른 집합이면) 두 집합을 합친다.
		=> root of x = find(x), root of y = find(y), 
	2. 합치는 방법은 1번에서 찾은 y의 루트노드의 부모로 x의 루트노드를 넣어준다.  
		=> parent\[find(y)\] = find(x)
		
3. **Find(x)**
	1. parent배열은 특정 인덱스 원소의 부모노드를 저장한다. 부모를 계속 따라가다 보면 자기자신을 부모로 가지는 원소가 존재하게 되는데 이 것이 해당 집합의 루트이다.
		=> <반복문> while parent\[x] == x : x = parent\[x] 
		=>  <재귀> if parent\[x] == x : x = find(parent\[x] ); return parent\[x]

### Code (python)

1. **Initialize** 
```python
parent = [i for i in range(N)]
```
2. **union(x,y)**
```python
def union(x,y):
	x_root = find(x)
	y_root = find(y)

	# 같은 집합이면 합칠 필요가 없다!
	if x_root == y_root:
		return

	parent[y_root] = x_root
```
3. **Find(x)**
```python
def find(x):
	if parent[x] == x:
		return parent[x]
	else
		return find(parent[x])
```

---
## 최적화

### 기존 Union-Find를 구현했을 때 성능

위와 같이 구현했을 때 **최악의 경우** 모든 트리의 요소가 한 가지를 가지는 **연결리스트 형태**가 되면 N개의 원소에 대해 모두 탐색해야 하므로 union과 find모두 O(N)만큼 걸린다.

### Union-Find 최적화 

#### 1. Find 최적화 - Path Compression (경로 압축)
find 함수는 현재 재귀함수로 루트임이 판단될 때 까지 계속 함수를 호출한다.  
이 때 방문한 노드들에 모두 루트를 넣어주는 것이 **Path Compression** 이라고 할 수 있다.

이렇게 하면 방문한 노드는 바로 부모로 바로 루트를 가지는 **Path Compression**(경로압축)이 되므로 이 후 이 노드들에 대해 find의 탐색시간이 줄게된다.

#### find(x) Code 수정
```python
def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])
    return parent[x]
```

#### 2.Union 최적화 - union-by-rank
![unionbyrank](unionbyrank.png)
Rank라는 배열을 도입해서 각 집합의 레벨을 표시한다.  
이렇게 해서 더 작은 Rank를 가지는 트리 집합이 더 큰 Rank를 가지는 트리집합에 들어가게 된다.

이렇게 한다면 더 큰 Rank를 가지는 트리 집합에 작은 Rank를 가지는 트리 집합이 붙기 때문에 UNION을 하더라도 트리의 깊이를 낮게 유지시킬 수 있다.

#### **union(x,y)** Code 수정

```python
def union_by_rank(v1, v2):
    p1 = find(v1)
    p2 = find(v2)

    if p1 == p2:
        return

    if rank[p1] > rank[p2]:  
        parent[p2] = p1
    elif rank[p1] < rank[p2]:
        parent[p1] = p2
    else:
        parent[p2] = p1
        rank[p1] += 1
```