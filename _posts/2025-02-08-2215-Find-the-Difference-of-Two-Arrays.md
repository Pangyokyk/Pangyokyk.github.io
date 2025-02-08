---
layout: post
title: 2215. Find the Difference of Two Arrays
tags: [Leetcode, Coding Test, hash table, easy]
date: 2025-02-08 +0800
math: true
toc : true
---



# 2215. Find the Difference of Two Arrays


****


## 문제

Given two 0-indexed integer arrays nums1 and nums2, return a list answer of size 2 where:

- answer[0] is a list of all distinct integers in nums1 which are not present in nums2.
- answer[1] is a list of all distinct integers in nums2 which are not present in nums1.


**Note** that the integers in the lists may be returned in any order.

 [문제 사이트](https://leetcode.com/problems/find-the-difference-of-two-arrays/description/?envType=study-plan-v2&envId=leetcode-75)

Example 1:
Input: nums1 = [1,2,3], nums2 = [2,4,6]
Output: [[1,3],[4,6]]
Explanation:
For nums1, nums1[1] = 2 is present at index 0 of nums2, whereas nums1[0] = 1 and nums1[2] = 3 are not present in nums2. Therefore, answer[0] = [1,3].
For nums2, nums2[0] = 2 is present at index 1 of nums1, whereas nums2[1] = 4 and nums2[2] = 6 are not present in nums2. Therefore, answer[1] = [4,6].


Example 2:
Input: nums1 = [1,2,3,3], nums2 = [1,1,2,2]
Output: [[3],[]]
Explanation:
For nums1, nums1[2] and nums1[3] are not present in nums2. Since nums1[2] == nums1[3], their value is only included once and answer[0] = [3].
Every integer in nums2 is present in nums1. Therefore, answer[1] = [].
 

Constraints:

1 <= nums1.length, nums2.length <= 1000
-1000 <= nums1[i], nums2[i] <= 1000



****


## 문제 해석
- 두 개의 인트형 벡터가 주어지는데 그중 각 유니크한 값들만 출력하는 문제이다.



****

## 구현

```cpp
class Solution {
public:
    vector<vector<int>> findDifference(vector<int>& nums1, vector<int>& nums2) {
        unordered_set set1(nums1.begin(), nums1.end());
        unordered_set set2(nums2.begin(), nums2.end());

        vector<int> result_num1, result_num2;

        for(int num : set1)
        {
            if(set2.count(num) == 0)
            {
                result_num1.push_back(num);
            }
        }
        for(int num : set2)
        {
            if(set1.count(num) == 0)
            {
                result_num2.push_back(num);
            }
        }

        return {result_num1, result_num2};
    }
};
```



****


## 코드 해석

```cpp
unordered_set set1(nums1.begin(), nums1.end());
unordered_set set2(nums2.begin(), nums2.end());
```

- hash table 중 **중복** 없는 값만을 가지고 있는 unordered_set이다.
- set1에 nums1의 처음부터 끝까지 중복없이 값들을 넣어주는 것이다.
- **순서는 완전히 랜덤이다**


```cpp
for(int num : set1)
```

- C++ 11버전에서 나온 문법이다.
- int형 num 에 set1 값들을 **하나** 씩 전부 순회한다고 생각하면 된다.
- 즉 num 에는 set1의 값들이 하나씩 전부 들어있고 그걸로 for문에 작성한 문법대로 작동한다는 것이다.


```cpp
if(set2.count(num) == 0)
{
    result_num1.push_back(num);
}
```

- num 에는 set1의 값들이 들어가있다.
- set2의 값들과 하나씩 비교해 가며 만약 count() == 0 즉, 존재하지 않는다면 그 숫자는 set1만의 유니크한 숫자이기 때문에 위 vector에 push_back을 이용해 값을 넣어주는 것이다.