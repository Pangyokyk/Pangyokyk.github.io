---
layout: post
title: 70. Climbing Stairs
tags: [Leetcode, Coding Test, dynamic programming, easy]
date: 2025-01-23 +0800
math: true
toc : true
---



# 70. Climbing Stairs


****


## 문제 
You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

 

Example 1:
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps


Example 2:
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
 

Constraints:

1 <= n <= 45


****


## 문제 해석
- 계단은 한번에 1개와 2개를 올라갈 수 있음
- 계단의 수는 1~45개 사이로 n개의 개단을 올라갈 수 있는 방법을 전부 다 세는 문제이다.


****


## 생각

- 일단 처음에 문제를 접했을 때는 수학적으로 접근해서 풀려고 했다.
- 규칙을 계산하니 피보나치 수열과 동일한 형태를 가지는 점화식이 나온다.
- Dynamic Programming 문제이다.


****


## 구현

```cpp
class Solution {
public:
    int climbStairs(int n) {
        if(n == 1)
        {
            return n;
        }

        vector<int> dp(n+1);

        dp[1] = 1;
        dp[2] = 2;

        for(int i = 3; i <= n; ++i)
        {
            dp[i] = dp[i-1] + dp[i-2];
        }

        return dp[n];
    }
}
```


****


## 코드 해석

- 바텀업 과정으로 풀은 코드이다.
- 탑다운(재귀사용)으로 푸니 오버플로우가 발생해 실패했다.
- 먼저 n == 1 (계단이 1개) 는 올라가는 방법이 1가지이다.
- 그리고 찾아낸 점화식이 피보나치 수열과 동일한 점화식임을 알고 있고 바텀업(반복문 사용)으로 코드를 작성했다.
- **vector를 적는 방법**
  1. vector<int> vc(10, -1); // 크기 10, -1로 초기화
  2. vector<int> vc(10) // 크기 10, 기본값 0으로 초기화
  3. vector<int> vc = {1,2,3,4,5} // 초기화와 동시에 선언



****

## 후기
Dynamic Programming은 문제가 주어질 때마다 규칙을 찾아내서 **점화식** 찾는 것이 중요한 듯하다. DP의 심화과정 알고리즘도 자주 출제된다고 하니 DP의 방식 탑다운(재귀)와 바텀업(반복문)을 꼭 기억하자
<mark>메모이제이션은 계산해둔 데이터를 저장해두는 방식을 말한다</mark>