---
layout: post
title: 229. Majority Element II
tags: [Leetcode, Coding Test, counting, hash table, medium]
date: 2025-04-14 +0800
math: true
toc : true
---


# 229. Majority Element II


Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times.

[문제 사이트](https://leetcode.com/problems/majority-element-ii/description/) 

Example 1:

Input: nums = [3,2,3]
Output: [3]

Example 2:
Input: nums = [1]
Output: [1]

Example 3:
Input: nums = [1,2]
Output: [1,2]
 

Constraints:

- 1 <= nums.length <= 5 * 10^4
- -10^9 <= nums[i] <= 10^9
 

**Follow up: Could you solve the problem in linear time and in O(1) space?**




## 문제 해석

- nums 의 배열에서 nums의 크기 n의 n/3보다 많이 등장하는 숫자들을 출력하는 것
- major 숫자는 배열의 크기의 1/3을 초과하여 등장해야하기 때문에 **major 숫자는 최대 2개까지 밖에 될 수 없다.**
- **Follow up은 이것을 시간, 공간 복잡도 선형시간 O(1)에 수렴하게 짜는 것이다.**




## 구현(hash table)

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        vector<int> ans;
        unordered_map<int, int>   mp;
        
        int n = nums.size();

        for(auto& num : nums)
        {
            mp[num]++;
        }

        for(auto& a : mp)
        {
            if(a.second > n/3)
            {
                ans.push_back(a.first);
            }
        }
        return ans;
    }
};
```


## 코드 해석

- 해당 풀이는 **Follow up 의 O(1) 알고리즘** 방법으로 풀지 못해 hash table 을 이용한 풀이이다.
- 이 풀이는 시간, 공간복잡도가 **O(n)** 이기 때문에 최적의 풀이라고 할 수 없다.





## 구현(Boyer–Moore majority vote algorithm)

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        vector<int> ans;
        int n = nums.size();

        int candidate1 = -1;
        int candidate2 = -1;

        int vote1 = 0;
        int vote2 = 0;

        for(auto& x : nums)
        {
            if(candidate1 == x) // x 가 후보자1과 같을 경우
            {
                vote1++;
            }
            else if(candidate2 == x) // x 가 후보자2과 같을 경우
            {
                vote2++;
            }
            else if(!vote1) // vote1 이 존재하지 않을 경우 = 후보자1이 존재하지 않을 경우
            {
                candidate1 = x;
                vote1 = 1;
            }
            else if(!vote2) // vote2 이 존재하지 않을 경우 = 후보자 2이 존재하지 않을 경우 
            {
                candidate2 = x;
                vote2 = 1;
            }
            else // x가 후보자1,2 와 다른 숫자일 경우
            {
                vote1--;
                vote2--;
            }
        }

        // 위에서는 후보자 최대 두명까지 골라냈는데
        // 이 후보자가 배열의 크기의 1/3 초과하여 등장했는지
        // 검사가 필요하다.
        vote1 = 0;
        vote2 = 0;

        for(auto& a : nums)
        {
            if(a == candidate1)
            {
                vote1++;
            }
            else if(a == candidate2)
            {
                vote2++;
            }
        }
        
        // 검증한 vote1,2가 n/3 보다 크다면 이제 정답 배열에 넣는다.
        if(vote1 > n/3)
        {
            ans.push_back(candidate1);
        }
        if(vote2 > n/3)
        {
            ans.push_back(candidate2);
        }

        return ans;
    }
};
```



## 코드 해석

- 위의 풀이는 **Boyer–Moore Algorithm(보이어-무어 알고리즘)** 이다.
- 이 알고리즘은 투표가 나올 때마다 현재 최고 투표 수의 후보자 투표 수를 깎으며 투표수가 0 이 되었을 때 후보자를 교체하여 최종적으로 최고 투표자를 뽑는 알고리즘이다.
- **시간 복잡도 = O(n)** - nums의 크기만큼 for문이 돌기 때문
- **공간 복잡도 = O(1)** - 정답 vector ans 는 최대 2개의 원소가 들어갈 수 있기 때문에 O(2) = O(1) 의 선형 공간이다.