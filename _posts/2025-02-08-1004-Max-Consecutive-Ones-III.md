---
layout: post
title: 1004. Max Consecutive Ones III
tags: [Leetcode, Coding Test, sliding window, medium]
date: 2025-02-06 +0800
math: true
toc : true
---



# 1004. Max Consecutive Ones III


****


## 문제

Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.

 [문제 사이트](https://leetcode.com/problems/max-consecutive-ones-iii/description/?envType=study-plan-v2&envId=leetcode-75)

Example 1:
Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6
Explanation: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.


Example 2:
Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10
Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
 

Constraints:

1 <= nums.length <= 10^5
nums[i] is either 0 or 1.
0 <= k <= nums.length



****


## 문제 해석

- nums 벡터에서 k 만큼 0을 1로 만들어서 최대 연속된 1의 갯수를 구하는 문제이다.



****

## 구현

```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int right;
        int left = 0;

        for(right = 0; right < nums.size(); right++)
        {
            if(nums[right] == 0)
            {
                k--;
            }
            if(k < 0)
            {
                if(nums[left] == 0)
                {
                    k++;
                }
                left++;
            }
        }
        return right - left;
    }
};
```


****


## 코드해석
```cpp
int right;
int left = 0;
```
- sliding window를 사용하기 위한 변수들
- 즉, 조건 내의 만족하는 범위를 지정하기 위한 변수들

```cpp
if(nums[right] == 0)
{
    k--;
}
```
- k 만큼 0을 1로 뒤집어야 한다.
- 0을 만나면 뒤집는 횟수를 사용한 것

```cpp
if(k < 0)
{
    if(nums[left] == 0)
    {
        k++;
    }
    left++;
}
```

- 문제를 푸는데 핵심적인 부분
- 만약 k번 만큼 0을 만났다면 k = 0 이 된다.
- 그 후 또 0을 만나면 k = -1이 되는데 문제 조건에 맞지 않기 때문에 범위를 맨 앞부터 조사해 nums[left] == 0 이 될때 까지 당긴다.

