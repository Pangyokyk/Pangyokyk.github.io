---
layout: post
title: 162. Find Peak Element
tags: [Leetcode, Coding Test, binary search, medium]
date: 2025-04-09 +0800
math: true
toc : true
---


# 162. Find Peak Element



## 문제 

A peak element is an element that is strictly greater than its neighbors.

Given a 0-indexed integer array nums, find a peak element, and return its index. If the array contains multiple peaks, return the index to any of the peaks.

You may imagine that nums[-1] = nums[n] = -∞. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

**You must write an algorithm that runs in O(log n) time.**

[문제 사이트](https://leetcode.com/problems/find-peak-element/description/?envType=study-plan-v2&envId=top-interview-150) 

Example 1:

Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
Example 2:

Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
 

Constraints:

- 1 <= nums.length <= 1000
- -2^31 <= nums[i] <= 2^31 - 1
- nums[i] != nums[i + 1] for all valid i.



## 문제 해석

- **peak** 란 양옆의 숫자보다 큰 자신을 말한다.
- 배열의 양옆 끝에는 -∞ 즉, 무한히 작은 숫자가 존재한다고 가정한다.
- **O(logn)** 의 시간복잡도를 가지는 알고리즘을 작성하라.



## 코드 구현

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int low = 0;
        int high = nums.size() - 1;

        while(low < high)
        {
            int mid = low + (high - low) / 2;

            if(nums[mid] < nums[mid+1])
            {
                low = mid + 1;
            }
            else
            {
                high = mid;
            }
        }
        return low;
    }
};
```

### <mark>해당 문제는 왜 Binary search(이진 탐색) 문제인가?</mark>

- 이진 탐색은 **정렬된 배열** 에서 사용할 수 있다고 알고 있었다.
- 하지만 이 문제처럼 조건이 주어진다면 **비정렬 배열** 에서도 사용할 수 있다.


이 문제에서 이진 탐색으로 풀라는 것을 알려준 조건은
1. **No adjacent two numbers are the same**
   - **인접한 두 숫자는 동일하지 않다.**
2. **the two end of the arrays are -∞**
   - **두 배열의 양 끝은 무한히 작은 숫자로 감싸여 있다**
3. **You can return any peak.**
   - **peak가 여러개 존재하면 어느 peak를 리턴해도 상관없다**
4. **You must write an algorithm that runs in O(log n) time.**
   - **시간복잡도 O(logn)의 알고리즘을 작성하라**


위 조건을 부여함으로서 비정렬 배열에 이진탐색을 적용할 수 있었던 것이다. 구글, 페이스북에서 자주 출제되었던 개념이라고 한다.


## 코드 해석


```cpp
if(nums[mid] < nums[mid+1])
```
- 일반적인 이진 탐색과 비슷한 구조이지만 비교하는 조건이 다르다.
- 만약 nums[mid] 가 바로 옆 nums[mid+1] 보다 작다면 **peak는 mid의 오른쪽 구역에 존재한다고 단언할 수 있다.**
- 만약 그 반대라면 nums[mid] 가 peak 가 될 수 있으므로 high = mid - 1 이 아닌 **high = mid** 로 구역을 좁혀준다.


**<mark>이 문제의 중요한 점은 비정렬 배열에서 조건이 주어진다면 이진 탐색을 적용할 수 있다는 개념이다.</mark>**
