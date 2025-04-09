---
layout: post
title: 74. Search a 2D Matrix
tags: [Leetcode, Coding Test, binary search, medium]
date: 2025-04-09 +0800
math: true
toc : true
---



# 74. Search a 2D Matrix



## 문제

You are given an m x n integer matrix matrix with the following two properties:

- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.


Given an integer target, return true if target is in matrix or false otherwise.

**You must write a solution in O(log(m * n)) time complexity.**

[문제 사이트](https://leetcode.com/problems/search-a-2d-matrix/description/?envType=study-plan-v2&envId=top-interview-150) 

Example 1:
![그림](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true


Example 2:
![그림](https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg)

Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false
 

Constraints:

- m == matrix.length
- n == matrix[i].length
- 1 <= m, n <= 100
- -10^4 <= matrix[i][j], target <= 10^4




## 문제 해석

- 2차원 배열이 주어지면 target 의 존재 여부를 확인하는 문제
- 이 문제의 핵심은 **시간 복잡도 O(logn)** 을 만족시켜야 한다는 것
- 즉, 배열에서 무언가를 찾는다 -> 문제 조건에 시간복잡도 O(logn) 이 주어졌다 = **이진 탐색 문제** 라는 소리이다.



## 구현

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int row = matrix.size();
        int col = matrix[0].size();

        int low = 0;
        int high = (row * col) - 1;

        while(low <= high)
        {
            int mid = low + (high - low) / 2;

            if(matrix[mid/col][mid%col] == target)
            {
                return true;
            }
            else if(matrix[mid/col][mid%col] < target)
            {
                low = mid + 1;
            }
            else
            {
                high = mid - 1;
            }
        }
        return false;
    }
};
```



## 코드해설

- 지금까지 **Binary search** 에 대한 문제는 정렬된 1차원 배열 조건에서 문제가 주어졌었다.
- 2차원 배열에서 이진 탐색을 적용하는 문제는 처음이라 접근하기 힘들었다.
- 코드는 거의 보통의 이진 탐색 문제와 동일하지만 **2차원 배열의 중간요소를 찾는 부분의 공식이였다.**

```cpp
if(matrix[mid/col][mid%col] == target)
```

- 2차원 배열 matrix[0]의 중간 지점은 mid를 col **나눈 값**
- 2차원 배열 matrix[1]의 중간 지점은 mid를 col 나눈 **나머지 값**
- 그래서 **matrix[mid/col][mid%col]** 라는 식을 모르면 풀 수 없던 문제였다.
- 외워두자. 이진 탐색에서 2차원 배열의 중간값을 찾는 공식은 행 = mid **/** 열, 열 = mid **%** 열 이라는 것을
