---
layout: post
title: 1207. Unique Number of Occurrences
tags: [Leetcode, Coding Test, hash table, easy]
date: 2025-02-09 +0800
math: true
toc : true
---




# 1207. Unique Number of Occurrences



****



## 문제

Given an array of integers arr, return true if the number of occurrences of each value in the array is unique or false otherwise.

 [문제 사이트](https://leetcode.com/problems/unique-number-of-occurrences/description/?envType=study-plan-v2&envId=leetcode-75)

Example 1:
Input: arr = [1,2,2,1,1,3]
Output: true
Explanation: The value 1 has 3 occurrences, 2 has 2 and 3 has 1. No two values have the same number of occurrences.

Example 2:
Input: arr = [1,2]
Output: false

Example 3:
Input: arr = [-3,0,1,-3,1,1,1,-3,10,0]
Output: true
 

Constraints:

1 <= arr.length <= 1000
-1000 <= arr[i] <= 1000



****


## 문제 해석
- 주어진 배열에서 각 숫자의 반복 횟수를 세서 그 반복 횟수가 고유한 숫자인지 판별하는 문제이다.



## 구현

```cpp
class Solution {
public:
    bool uniqueOccurrences(vector<int>& arr) {
        unordered_map<int, int> umap;
        unordered_map<int> uset;

        for(auto num : arr)
        {
            umap[num]++;
        }
        for(auto i : umap)
        {
            if(!uset.insert(i.second).second)
            {
                return false;
            }
        }
        return true;
    }
};
```



****


## 코드 해석

```cpp
unordered_map<int, int> umap;
unordered_map<int> uset;
```

- unordered_map 은 key-value(키-값) 을 쌍으로 가지고 있는 해쉬테이블
- unordered_set 은 value만 가지고 있는 해쉬테이블

```cpp
for(auto num : arr)
        {
            umap[num]++;
        }
```

- for(auto num : arr) 은 자료형 auto(자동으로 인식해줌) num에 arr의 요소들을 순서대로 하나씩 넣어본다는 뜻
- **<mark>umap[num]++; 의 해석은 이러하다</mark>**
  1. umap은 int형 키와 int형 값을 하나의 쌍으로 저장하는 해쉬테이블
  2. num은 vector arr의 요소를 가지고 있다.
  3. 값이 들어올때 마다 key(arr 요소) 와 value(요소가 몇번 들어왔는지) 를 저장한다.
  4. 만약 vector<int> arr = [1, 2, 2, 1, 1, 3] 이라면 umap = { 1(값) : 3(중복횟수), 2 : 2, 3 : 1 } 이렇게 형성된다는 것이다.


```cpp
for(auto i : umap)
        {
            if(!uset.insert(i.second).second)
            {
                return false;
            }
        }
```

- auto 형 i에 umap을 하나씩 넣어본다.
- **insert()** 는 삽입 성공 여부를 pair<bool, iterator> 로 반환한다.
- 즉 .second 가 true면 삽입 성공, false면 삽입 실패라는 것이다.
- i.second 는 숫자 중복 횟수를 나타냄
- 거기에 insert().second -> 삽입에 성공했는지 안했는지 여부 확인
- 만약 **실패했다 = 중복이다** 이므로 바로 false를 리턴한다.