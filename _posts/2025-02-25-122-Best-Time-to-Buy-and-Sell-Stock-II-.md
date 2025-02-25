---
layout: post
title: 122. Best Time to Buy and Sell Stock II
tags: [Leetcode, Coding Test, array, dynamic programming, greedy, medium]
date: 2025-02-25 +0800
math: true
toc : true
---



# 122. Best Time to Buy and Sell Stock II


****


## 문제

You are given an integer array prices where prices[i] is the price of a given stock on the ith day.

On each day, you may decide to buy and/or sell the stock. You can only hold at most one share of the stock at any time. However, you can buy it then immediately sell it on the same day.

Find and return the maximum profit you can achieve.

[문제 사이트](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/?envType=study-plan-v2&envId=top-interview-150) 

Example 1:

Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.

Example 2:

Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.

Example 3:

Input: prices = [7,6,4,3,1]
Output: 0
Explanation: There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.
 

Constraints:

1 <= prices.length <= 3 * 10^4
0 <= prices[i] <= 10^4


****


## 문제 해석
- 121번 문제와 같이 주식의 매수, 매도 최고 이익을 묻는 문제이다.
- 다만, 한번의 최대 이익이 아니라 한 주를 팔면 또 사서 이익을 여러번 실현할 수 있다는 것이 다른 점이다.



****


## 구현

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxpro = 0;
        int minprice = prices[0];

        for(int i = 1; i < prices.size(); i++)
        {
            if(prices[i] > minprice)
            {
                maxpro += prices[i] - minprice;
            }
            minprice = prices[i];
        }
        return maxpro;
    }
};
```


****


## 코드 해석


```cpp
for(int i = 1; i < prices.size(); i++)
{
    if(prices[i] > minprice)
    {
        maxpro += prices[i] - minprice;
    }
    minprice = prices[i];
}
```

- **문제 해석을 너무 어렵게 생각하는 경향이 있는것 같다.**
- 생각해보면 최대 이익을 실현하려면 매수해서 그 매수한 값보다 비쌀 때 팔기를 반복하면 되는 것이다.
- **<mark>최대</mark> 이익 실현 > Greedy 문제구나** 이런 생각의 흐름진행이 자연스럽게 이어져야 한다고 leetcode 토론에 써 있었다.