---
layout: post
title: 27. Remove Element
tags: [Leetcode, Coding Test, vector, easy]
date: 2025-01-22 +0800
math: true
toc : true
---



# 27. Remove Element


****


# 문제 

Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:

- Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
- Return k.

**Custom Judge:**

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
If all assertions pass, then your solution will be accepted.


****


## 문제 해석
- Input으로 vector<int> nums 와 삭제하고자 하는 val 을 입력받아 삭제 후 nums의 크기를 물어보는 문제이다.


****


## 생각
입력받은 val을 삭제 해야함 > 반복문을 돌려서 nums[i] == val 이면 삭제시키기 > 제거 후 배열을 어떻게 당길까?


****


# 구현

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {

        for(int i =0; i < nums.size(); ++i)
        {
            if(nums[i] == val)
            {
                nums.erase(nums.begin()+ i);
                --i;
            }
        }

        return nums.size();

    }
};
```


****


## 코드 해석

```cpp
if(nums[i] == val)
{
    nums.erase(nums.begin()+ i);
}
```
- 처음에는 if 조건문을 이렇게 작성했다. 하지만 **nums = [0,1,2,2,3,0,4,2], val = 2** 인 Input이 들어왔을 때 **[0,1,3,0,4,2]** 로 '2'가 하나 지워지지 않았다.
- 뭔가 반복문을 도는데 범위가 문제가 있구나라는 생각이 빠르게 들었다.
- 문제는 **<mark>배열이 지워지면 배열의 요소들이 앞으로 이동</mark>** 을 생각하지 않았기 때문이였다.
- 맨 처음 '2'가 지워질 때 nums = [0,1,2,3,0,4,2]로 한칸씩 요소들이 이동하는데 i = 2 에서 i = 3 으로 증가하기 때문에 그 다음 num[3] = 2 검사를 건너뛰어 삭제가 되지 않았던 것이다.
- 그렇기 때문에 만약 삭제를 하게 된다면 증가하는 i를 한번 감소시켜줘야한다.