---
layout: post
title: 1679. Max Number of K-Sum Pairs
tags: [Leetcode, Coding Test, two pointers, sorting, medium]
date: 2025-02-06 +0800
math: true
toc : true
---



# 1679. Max Number of K-Sum Pairs


****


# 문제
You are given an integer array nums and an integer k.

In one operation, you can pick two numbers from the array whose sum equals k and remove them from the array.

Return the maximum number of operations you can perform on the array.

[문제사이트](https://leetcode.com/problems/max-number-of-k-sum-pairs/description/?envType=study-plan-v2&envId=leetcode-75)
 
Example 1:
Input: nums = [1,2,3,4], k = 5
Output: 2
Explanation: Starting with nums = [1,2,3,4]:
- Remove numbers 1 and 4, then nums = [2,3]
- Remove numbers 2 and 3, then nums = []
There are no more pairs that sum up to 5, hence a total of 2 operations.


Example 2:
Input: nums = [3,1,3,4,3], k = 6
Output: 1
Explanation: Starting with nums = [3,1,3,4,3]:
- Remove the first two 3's, then nums = [1,4,3]
There are no more pairs that sum up to 6, hence a total of 1 operation.
 

Constraints:
1 <= nums.length <= 10^5
1 <= nums[i] <= 10^9
1 <= k <= 10^9


****


## 문제 해석
- nums 벡터에서 두 개의 숫자를 골라 int형 k 를 몇개 만들 수 있는지 묻는 문제이다.
- 만약 두 개를 골라 k를 만들었다면 삭제를 해야한다.


```cpp
class Solution {
public:
    int maxOperations(vector<int>& nums, int k) {
        int left = 0;
        int right = nums.size()-1;
        int count = 0;

        sort(nums.begin(), nums.end());

        while(left < right)
        {
            if(nums[left] + nums[right] == k)
            {
                count++;
                left++;
                right--;
            }
            else if(nums[left] + nums[right] < k)
            {
                left++;
            }
            else
            {
                right--;
            }
        }

        return count;
    }
};
```


****


## 코드 해석

```cpp
int left = 0;
int right = nums.size()-1;
int count = 0;
```

- 두 개의 포인터를 이용하기 위한 왼쪽 끝 **left** 와 오른쪽 끝 **right**
- 그리고 k를 조합 가능한 쌍이 몇개 있는지 세기 위한 변수 count

```cpp
sort(nums.begin(), nums.end());
```
- 이번 문제에서 가장 떠올리기 힘든 코드
- 정렬을 시키지 않으면 Two Pointer 를 사용하지 못한다.
- sort를 사용하면 시간 복잡도는 \(O(nlogn\)) 이 되는데 이를 \(O(n\)) 으로 줄일 수 있는 hash table 을 사용한 방법이 있었지만 보니 아직 내 수준으로는 생각조차 할 수 없어보이기에 생략하겠다.


```cpp
if(nums[left] + nums[right] == k)
{
    count++;
    left++;
    right--;
}
else if(nums[left] + nums[right] < k)
{
    left++;
}
else
{
    right--;
}
```

- 위에서 sort를 사용하여 오름차순으로 정리했기 때문에 가능한 코드이다.


****


## 후기
- 문제를 풀 때 핵심적인 아이디어 떠오르는 훈련을 더해야한다.

