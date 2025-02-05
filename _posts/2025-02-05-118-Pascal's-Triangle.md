---
layout: post
title: 118. Pascal's Triangle
tags: [Leetcode, Coding Test, array, dynamic programming, easy]
date: 2025-02-05 +0800
math: true
toc : true
---



# 118. Pascal's Triangle


****

## 문제

Given an integer numRows, return the first numRows of Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

![파스칼 삼각형](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)
 

Example 1:
Input: numRows = 5
Output: [[[[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]]]

Example 2:
Input: numRows = 1
Output: [[[1]]]
 

Constraints:

- 1 <= numRows <= 30



****

## 문제 해석
- 파스칼 삼각형을 구현해보는 문제이다.


****


## 코드 구현(재귀적 방법)

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        if(numRows == 0)    return {};
        if(numRows == 1)    return {{1}};

        vector<vector<int>> newprev = generate(numRows - 1);
        vector<int> newRows(numRows, 1);

        for(int i = 1; i < numRows - 1; ++i)
        {
            newRows = newprev.back()[i - 1] + newprev.back()[i];
        }

        newprev.push_back(newRows);
        return newprev;
    }
};
```


****


## 코드 해석

```cpp
if(numRows == 0)    return {};
if(numRows == 1)    return {{1}};
```
- 파스칼 삼각형 특징으로 첫번째 배열 고정

```cpp
vector<vector<int>> newprev = generate(numRows - 1);
vector<int> newRows(numRows, 1);
```
- 재귀적인 방법으로 정답을 담을 newprev 객체 생성과 파스칼 삼각형 계산을 위한 newRows 배열 생성(1로 초기화)

```cpp
for(int i = 1; i < numRows - 1; ++i)
{
    newRows = newprev.back()[i - 1] + newprev.back()[i];
}
```
- 파스칼 공식을 사용한 점화식이다.
- back() 메소드는 맨 뒤에 값을 불러온다.(수정 가능)



****


## 코드 구현(반복적 방법)

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> result;
        for (int i = 0; i < numRows; i++) {
            vector<int> row(i + 1, 1);
            for (int j = 1; j < i; j++) {
                row[j] = result[i - 1][j - 1] + result[i - 1][j];
            }
            result.push_back(row);
        }
        return result;
    }
};
```