---
layout: post
title: 26. Remove Duplicates from Sorted Array
tags: [Leetcode, Coding Test, vector, easy]
date: 2025-01-21 +0800
math: true
toc : true
---


# 26. Remove Duplicates from Sorted Array


****


## 문제

Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in nums.

Consider the number of unique elements of nums to be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the unique elements in the order they were present in nums initially. The remaining elements of nums are not important as well as the size of nums.
- Return k.

**Custom Judge:**

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
If all assertions pass, then your solution will be accepted.

Example 1:

Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
Example 2:

Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).


****


## 문제 해석
- 배열에 관한 문제로 중복된 문자를 지우고 고유의 숫자만 세는 문제이다.
- Output으로 고유의 숫자 갯수와 중복된 숫자는 지운 배열을 출력해줘야한다.


****


## 생각
일단 문제를 풀기 전 내가 생각한 것들이다.
1. 배열의 중복을 제거해야함.
2. 고유 숫자의 갯수를 어떻게 셀 것인지

**배열을 하나씩 검사 > 숫자(count)세기 > 고유 숫자 배열에 넣어놓기 > 중복 들어오면 어떻게 처리??**


****


## 구현

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int count = 1;

        for(int i = 0; i < nums.size() - 1; i++)
        {
            if(nums[i] != nums[i + 1])
            {
                nums[count] = nums[i + 1];
                count++;
            }
        }

        return count;
        
    }
};
```


****


## 코드 관찰
**1. 고유 숫자들을 어떻게 셀 것인가?**
   - 일단 int형 변수를 선언해서 거기에 저장시켜야한다.
   - '처음 초기화를 '0'으로 해놓는 것이 맞는가?' 에 대해 많이 생각했는데 배열 처음의 인덱스는 무조건 고유의 숫자로 취급해서 그 다음 인덱스를 비교해서 고유 숫자를 세야 하기 때문에 count를 '1'로 초기화 하는 것이 맞다.

**2. count의 증가 조건**
   - count는 nums[i] 와 nums[i + 1] 이 다를 때만 증가시킨다(둘이 다르다는 건 새로운 숫자가 들어왔다는 뜻이기 때문)

**3. 배열 변경**
   - 고유 숫자 세는 건 해결했으니 배열을 어떻게 변경할 것인지 많은 고민을 했다.
   - 처음에는 앞으로 어떻게 땡겨올 것인지에 생각했는데 이러한 생각은 잘못됐다고 깨달았다.
   - 바로 nums[i]와 nums[i + 1]이 달랐을 때 고유 숫자로 판명되어 배열에 숫자가 저장된다는 것이다. 그리고 이건 count가 가지고 있는 값이다.
   - count의 초기 값은 1로 처음 고유 숫자는 배열에 저장시킴,  **<mark>count값이 증가한다 = 새로운 고유숫자 등장</mark>** 이라는 의미이기 때문이다.



****


## 후기
내가 처음으로 생각과 답까지 맞춘 문제이다. 뭔가 문제가 easy중의 easy 인 모양이다. 포인터, 주소, 함수 뭐 이런 개념없이 그냥 컴퓨팅적 사고만 가지고 있다면 풀 수 있었기 때문이다. 앞으로 더욱 공부해서 hard까지 스스로 풀어보고 싶다.