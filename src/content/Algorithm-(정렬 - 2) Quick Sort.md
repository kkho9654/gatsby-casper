---
layout: post
title: ' [Algorithm](정렬-2)QuickSort'
author: [KKH]
tags: ['Algorithm', '알고리즘', '정렬', 'QuickSort', '병합정렬']
image: img/Algorithm.png
date: '2023-10-05T22:10:39.341Z'
draft: false
---
# 정렬 시리즈 2 - Quick Sort
```toc
```
## Quick Sort
분할정복(divide and conquer)을 사용하는 정렬 알고리즘이다.  
> *분할정복이란 어떤 문제를 두개로 나누어서 각각 해결한 후 결과를 모아 원래의 문제를 해결하는 방식으로, 주로 재귀적 호출을 통해 구현한다.*

MergeSort와 동작방식은 비슷하나 QuickSort는 부분정렬을 pivot을 기준으로 비균등하게 분배한다

## Quick Sort 동작 과정
하나의 리스트를 두개로  나눈 후 각각 정렬한다.  
이후 각각 정렬된 두 리스트를 합쳐서 하나의 정렬된 리스트로 만든다.

### Quick Sort 단계
- Divide : 리스트를 피벗을 기준으로 비균등하게 2개의 부분 배열로 나눈다.
- Conquer : 나눈 부분 배열을 정렬한다. 부분 배열의 크기가 최대한 작아질 때 까지 재귀호출한다.
- Combine : 두개의 부분 정렬을 합친다. 

##  Quick Sort 구현 (python)


### Initialize
정렬되지 않은 리스트 선언
```python
# list선언
arr = [8,3,7,2,6,9,1,4]
```

### Quick Sort 함수 정의
```python
def divide(arr, left, right):
	pivot = arr[left]
	pivot_idx = left
	left+=1
	while left <= right:
		while arr[left] < pivot:
			left+=1
		while arr[right] > pivot:
			right-=1
		if left <= right:
			arr[left], arr[right] = arr[right], arr[left]
			left, right = left+1, right-1
	arr[pivot_idx], arr[right] = arr[right], arr[pivot_idx]
	
	return left

# Quick Sort
def quick_sort(arr, left, right):
	if right-left<2:
		return
	pivot_idx = divide(arr, left, right) # Divide
	quick_sort(arr, left, pivot_idx-1) # Conquer
	quick_sort(arr, pivot_idx, right) # Conquer
```


### 실행
```python
quick_sort(arr, 0, len(arr)-1)

print(arr)
```

#### Result
```Bash
$ python3 quicksort.py
[1, 2, 3, 6, 7, 8, 9]
```
