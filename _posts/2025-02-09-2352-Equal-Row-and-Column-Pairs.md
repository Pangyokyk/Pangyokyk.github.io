---
layout: post
title: 2352. Equal Row and Column Pairs
tags: [Leetcode, Coding Test, hash table, medium]
date: 2025-02-09 +0800
math: true
toc : true
---



# 2352. Equal Row and Column Pairs


****


## 문제

Given a 0-indexed n x n integer matrix grid, return the number of pairs (ri, cj) such that row ri and column cj are equal.

A row and column pair is considered equal if they contain the same elements in the same order (i.e., an equal array).

 [문제 사이트](https://leetcode.com/problems/equal-row-and-column-pairs/description/?envType=study-plan-v2&envId=leetcode-75)

Example 1:

![문제1](https://assets.leetcode.com/uploads/2022/06/01/ex1.jpg)
Input: grid = [[3,2,1],[1,7,6],[2,7,7]]
Output: 1
Explanation: There is 1 equal row and column pair:
- (Row 2, Column 1): [2,7,7]



Example 2:

![문제2](https://assets.leetcode.com/uploads/2022/06/01/ex2.jpg)
Input: grid = [[3,1,2,2],[1,4,4,5],[2,4,2,2],[2,4,2,2]]
Output: 3
Explanation: There are 3 equal row and column pairs:
- (Row 0, Column 0): [3,1,2,2]
- (Row 2, Column 2): [2,4,2,2]
- (Row 3, Column 2): [2,4,2,2]
 

Constraints:

n == grid.length == grid[i].length

1 <= n <= 200

1 <= grid[i][j] <= 10^5



****


## 문제 해석
- 배열의 행과 열이 정확하게 일치하는 짝의 개수를 세는 문제이다.



****


## 구현

```cpp
class Solution {
public:
    int equalPairs(vector<vector<int>>& grid) {
        map<vector<int>, int> mp;
        int result = 0;

        for(auto a : grid)
        {
            mp[a]++;
        }
        for(int j = 0; j < grid[0].size(); j++)
        {
            vector<int> vec;
            for(int i = 0; i < grid.size(); i++)
            {
                vec.push_back(grid[i][j]);
            }
            result += mp[vec];
        }
        return result;
    }
};
```


****


## 코드 해석

```cpp
map<vector<int>, int> mp;
```
- **map** 과 unordered_map 은 키-값 쌍을 저장한다는 공통점이 있지만 중요한 차이점도 존재한다.
- **map**은 **정렬될 상태** 로 저장이 된다는 것
- **행이 정렬된 상태** 가 되야 열과 정확한 비교가 가능하기 때문에 이 문제에서는 **map**을 사용한 것이다.


```cpp
for(auto a : grid)
        {
            mp[a]++;
        }
```

- a에 grid를 하나씩 넣어 mp에 저장하는 식이다
- mp는 { 키(행) : 값(중복된 횟수, ...)} 식으로 저장되는 것이다.


```cpp
for(int i = 0; i < grid.size(); i++)
            {
                vec.push_back(grid[i][j]);
            }
```
- vec에 j번째의 열을 저정한다.

```cpp
result += mp[vec];
```

- **이 문제에서 가장 핵심적인 코드**

```cpp
grid = [[3,1,2,2],[1,4,4,5],[2,4,2,2],[2,4,2,2]]
``` 
- grid가 이렇게 생겼다면

```cpp
mp = {
    {[3, 1, 2, 2]: 1},
    {[1, 4, 4, 5]: 1},
    {[2, 4, 2, 2]: 2}
}
```

- mp는 이런식의 형태로 저장될 것이다.

```cpp
vec = [3,1,2,2]
```
- 첫번째 열은 vec에 이러한 형태로 저장될 것이다.
- mp에 동일한 형태가 존재한다.
- 동일한 형태가 존재하고 그 value(값, 중복횟수)는 **1** 이므로 ans = 1 이 되는 것이다.