---
title: "자료구조 - 시간 복잡도 분석"
date: 2025-11-09 13:00:00 +0900
categories: [자료구조]
tags: [알고리즘, 시간복잡도, Big-O]
---

## 시간 복잡도 (Time Complexity)

알고리즘의 효율성을 분석하는 가장 기본적인 방법입니다.

### Big-O 표기법

가장 일반적으로 사용되는 점근 표기법입니다:

$$O(1) < O(\log n) < O(n) < O(n \log n) < O(n^2) < O(2^n) < O(n!)$$

### 예제: 배열에서 최댓값 찾기

```python
def find_max(arr):
    max_val = arr[0]
    for num in arr:  # O(n)
        if num > max_val:
            max_val = num
    return max_val

# 시간 복잡도: O(n)
# 공간 복잡도: O(1)
```

### 예제: 이진 탐색

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:  # O(log n)
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1

# 시간 복잡도: O(log n)
# 공간 복잡도: O(1)
```

### 정렬 알고리즘 비교

| 알고리즘 | 최선 | 평균 | 최악 | 공간 복잡도 |
|---------|------|------|------|------------|
| 버블 정렬 | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ |
| 퀵 정렬 | $O(n \log n)$ | $O(n \log n)$ | $O(n^2)$ | $O(\log n)$ |
| 병합 정렬 | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ |

효율적인 알고리즘 설계를 위해서는 시간 복잡도 분석이 필수입니다! 🚀
