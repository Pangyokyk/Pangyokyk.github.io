---
layout: post
title: 121. Best Time to Buy and Sell Stock
tags: [Leetcode, Coding Test, array, dynamic programming, easy]
date: 2025-02-25 +0800
math: true
toc : true
---



# 121. Best Time to Buy and Sell Stock



****


## 문제

You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

[문제 사이트](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/?envType=study-plan-v2&envId=top-interview-150)

Example 1:

Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.


Example 2:

Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
 

Constraints:

1 <= prices.length <= 10^5
0 <= prices[i] <= 10^4



****


## 문제 해석
- 주식 매입, 매도에서 최고 수익을 찾는 문제



****



## 구현(self)

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        long long profit = 0;
        long long cur_price = 0;
        long long maxprice = 0;

        for(int i = 0; i < prices.size(); i++)
        {
            cur_price = prices[i];
            for(int j = i + 1; j < prices.size(); j++)
            {
                profit = prices[j] - cur_price;
                maxprice = max(maxprice, profit);
            }
        }

        if(maxprice > 0)
        {
            return maxprice;
        }
        else
        {
            return 0;
        }
    }
};
```


****


## 코드 해석
- 그냥 문제를 보자마자 이중 for문을 사용하면 풀 수는 있겠다라고 생각했다.
- 하지만 \(O(n^2\)) 인 시간복잡도를 가진 풀이방법은 매우 좋지않다.
- 당연히 해당 답안은 testcase 에서 Time Limit 에 걸려 오답처리 되었다.



****


## 구현


```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxpro = 0;
        int minprice = INT_MAX;

        for(int i = 0; i < prices.size(); i++)
        {
            minprice = min(minprice, prices[i]);
            maxpro = max(maxpro, prices[i]- minprice);
        }

        return maxpro;
    }
};
```



****



## 코드 해석

```cpp
for(int i = 0; i < prices.size(); i++)
{
    minprice = min(minprice, prices[i]);
    maxpro = max(maxpro, prices[i]- minprice);
}
```

- **이 문제 풀이방법의 핵심은 최솟값과, 최댓값을 계속 들고가서 비교-변경을 수행하는 것이였다.**

