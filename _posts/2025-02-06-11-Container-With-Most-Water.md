---
layout: post
title: 11. Container With Most Water
tags: [Leetcode, Coding Test, two pointers, greedy, medium]
date: 2025-02-06 +0800
math: true
toc : true
---



# 11. Container With Most Water


****

## 문제

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

 [문제 사이트](https://leetcode.com/problems/container-with-most-water/description/?envType=study-plan-v2&envId=leetcode-75)

Example 1:

![그림](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.


Example 2:
Input: height = [1,1]
Output: 1
 

Constraints:
n == height.length
2 <= n <= 10^5
0 <= height[i] <= 10^4


****

## 문제 해석
- height 벡터의 값은 높이를 나타내고 그 중 두 인덱스를 골라 그걸로 만들 수 있는 사각형 중 최대 크기의 사각형을 구하는 문제이다.



****


## 구현

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0;
        int right = height.size() - 1;
        int maxwater = 0;
        int curwater = 0;

        while(left < right)
        {
            curwater = (right - left) * min(height[left], height[right]);
            maxwater = max(maxwater, curwater);

            if(height[left] < height[right])
            {
                left++;
            }
            else
            {
                right--;
            }
        }

        return maxwater;
    }
};

```


****


## 코드 해석

- **Two Pointer** 문제로 접근해야한다.

```cpp
int left = 0; // 왼쪽 끝 포인터
int right = height.size() - 1; // 오른쪽 끝 포인터
int maxwater = 0; // 최대 물양(사각형 넓이) 변수
int curwater = 0; // 가장 최근 물양값 저장할 변수(비교해서 큰값 을 찾아야함)
```

- 필요한 변수들이다.(내가 자꾸 어떠한 값들을 비교할 때 값들 저장하는 방식을 너무 어렵게 생각해서 놓치는 부분이 있다는 것을 발견했다.)

```cpp
while(left < right)
```
- **Two Pointer** 문제에서 자주 쓰이는 조건이다.
- 왼쪽 끝과 오른쪽 끝에서 안으로 파고드는 방식이기 때문에 left 와 right가 같아지는 순간은 중복이기 때문에 없어야한다.

```cpp
curwater = (right - left) * min(height[left], height[right]);
maxwater = max(maxwater, curwater);
```
- 물양 구하는 방법과 최대 물양을 가져가기 위한 max 메소드이다.
- max를 이용해서 가장 최근에 계산한 curwater 과 maxwater를 비교해서 큰 값이 새로 나오면 계속 교체해나간다.

```cpp
if(height[left] < height[right])
{
    left++;
}
else
{
    right--;
}
```
- **이번 문제에서 생각하기 어려웠던 부분**
- 가장 큰 사각형을 찾기 위해서는 **높이**가 중요하므로 가장 높은 height의 인덱스 값을 찾기 위한 조건이다.