---
layout: post
title: 35. Search Insert Position
tags: [Leetcode, Coding Test, binary search, easy]
date: 2025-01-23 +0800
math: true
toc : true
---



# 35. Search Insert Position


****


## 문제
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:
Input: nums = [1,3,5,6], target = 5
Output: 2

Example 2:
Input: nums = [1,3,5,6], target = 2
Output: 1

Example 3:
Input: nums = [1,3,5,6], target = 7
Output: 4
 
Constraints:

1 <= nums.length <= 10^4
-10^4 <= nums[i] <= 10^4
nums contains distinct values sorted in ascending order.
-10^4 <= target <= 10^4


****


## 문제 해석
target을 입력받으면 vector<int> nums에서 해당 target의 인덱스를 반환하거나 만약 존재하지 않는다면 오름차순 기준으로 target을 배열에 넣어 그 인덱스 번호를 반환하면 되는 문제이다.


****


## 생각
1. 만약 target이 nums에 존재할 경우
2. 만약 target이 nums에 존재하지 않을 경우

이렇게 두가지로 나누어서 생각해 볼 수 있다. 
- **1** 의 경우 반복문을 돌려 숫자들의 크기를 비교해 target보다 더 큰 숫자가 나올 때 그 전 인덱스 번호를 출력하면 될 것이다. 
- **2** 의 경우 찾지 못한다면 target이 들어갔을 때의 -1 인덱스와 +1 인덱스 자리를 찾아 target의 인덱스를 찾아야 할 것이다.


****


## 구현

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int low = 0;
        int high = nums.size() - 1;

        //만약 nums에 존재하지 않는 target이
        //가장 큰값보다 더 크다면 nums 끝 인덱스에 배치됨
        if(nums[high] < target)
        {
            return high + 1;
        }

        while(low <= high)
        {
            int mid = low + high / 2;

            //만약 nums의 중간값과 target이 같으면
            //내가 찾고자 하는 인덱스 값이니 mid 리턴
            if(nums[mid] == target)
            {
                return mid;
            }
            //중간값이 target보다 작으면 중간값의 오른쪽 구역으로 범위 좁힘
            else if(nums[mid] < target)
            {
                low = mid + 1;
            }
            //중간값이 target보다 크면 중간값의 왼쪽 구역으로 범위 좁힘
            else
            {
                high = mid - 1;
            }
        }
        //만약 target이 nums에 존재하지 않는다면
        // while이 최대한 범위를 좁혀나간 자리에 존재해야할 숫자임
        return low;


    }

};

```


****


## 코드 해석
- 유명한 이진 탐색 binary search에 관한 문제이다
- 문제에서 \(O(logn\)) 의 시간 복잡도가 왜 주어졌나 생각해보지 못했는데 난이도가 easy이니 이진 탐색으로 풀라는 뜻이였다.
- 문제에서 **정렬된 배열** 이 조건이였다. 이걸 보고 이진 탐색을 사용해야 한다는 생각이 들어야할 것 같다.