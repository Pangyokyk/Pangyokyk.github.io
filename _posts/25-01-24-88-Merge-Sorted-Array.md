---
layout: post
title: 88. Merge Sorted Array
tags: [Leetcode, Coding Test, array, easy]
date: 2025-01-24 +0800
math: true
toc : true
---




# 88. Merge Sorted Array



****


## 문제 

You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

 

Example 1:
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

Example 2:
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].

Example 3:
Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.
 

Constraints:

nums1.length == m + n
nums2.length == n
0 <= m, n <= 200
1 <= m + n <= 200
-10^9 <= nums1[i], nums2[j] <= 10^9
 

**Follow up: Can you come up with an algorithm that runs in O(m + n) time?**



****


## 문제 해석
- nums1 과 nums2 는 비오름차순으로 정렬되어있음
- m 은 nums1의 0을 제외한 유효 인덱스 갯수, n은 nums2의 유효 인덱스 갯수이다.
- nums1의 0으로 채워진 자리를 nums2 배열의 숫자들로 merge하는 문제이다.


****


## 생각

- nums1의 m은 0을 제외해하고 센 갯수임
- nums1의 크기는 m+n임
- 그냥 nums1과 nums2를 병합한 다음에 sort() 메소드를 사용하면 될 것 같다.


****


## 구현(1)

```cpp
class Solution {
public :
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        for(int i = m, j = 0; j < n; ++j)
        {
            nums1[i] = nums2[j];
            i++
        }

        sort(nums1.begin(), nums1.end());
    }
};
```


****


## 코드 해석
평범하게 nums1의 0으로 채워진 인덱스부터 해서 nums2의 인덱스로 매칭시켜 숫자를 넣은 후 sort() 메소드를 사용해 정렬한 코드이다. 하지만...

**<mark>Follow up: Can you come up with an algorithm that runs in O(m + n) time?</mark>**

- **알고리즘 시간 복잡도를 \(O(m+n\)) 으로 맞춰 구현해보시오**

만약 sort() 메소도를 사용한다면 시간 복잡도는 \(O((m+n)log(m+n\))) 이 된다. \(O(m+n\)) 의 시간 복잡도를 맞추기 위해서는 sort() 메소드를 사용하지 말고 두 개의 포인터를 사용해 구현해야한다.



****


## 구현(2)

```cpp
class Solution {
public : 
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;

        while(j >= 0)
        {
            if(i >= 0 && nums1[i] > nums2[j])
            {
                nums1[k--] = nums[i--];
            }
            else
            {
                nums1[k--] = nums[j--];
            }
        }

    }
};
```


****


## 코드 해석

- 인덱스 번호를 표시하기 위한 i, j, k
- Example 1, 2 처럼 한 쪽이 비어있는 경우를 대비한 조건문
  - j >= 0 과 i >= 0
- 조건문을 통해 nums1[k] (가장 뒤 인덱스)부터 채워주면 된다.

```cpp
if(i >= 0 && nums1[i] < nums2[j])
{
    nums1[k--] = nums[j--];
}
else
{
    nums1[k--] = nums[i--];
}
```

- 조건문의 순서를 바꾼다면 **오버플로우** 발생
  - i가 0보다 작아지는 경우가 발생하여 잘못된 인덱스를 참조할 수 있기 때문


****


## 후기

내가 짠 코드보다 더 나은 시간 복잡도를 가지는 코드들이 존재한다. 꼭 이 경우도 공부해서 '어떻게하면 이런 생각을 할 수 있을까?' 라는 생각이 실현되게 해야겠다. 그리고 오버플로우가 발생하는 경우도 항상 생각해야한다.