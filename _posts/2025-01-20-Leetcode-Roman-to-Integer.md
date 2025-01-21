---
layout: post
title: Roman to Integer
tags: [Leetcode, Coding Test, hash table ]
date: 2025-01-20 +0800
math: true
toc : true
---

# Leetcode : Roman to Integer

## 문제 
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer.

## 문제 해석
Input으로 I, V, X, L, C, D, M 문자열 여러개를 받으면 그 문자열에 배치에 따라 달라지는 계산을 구현하라는 문제이다.
붙어있는 문자 두개를 비교하여 **<mark>왼쪽이 오른쪽보다 더 크면</mark>** 두 문자의 대치되는 숫자들을 더해주면 되고 **<mark>왼쪽이 오른쪽보다 더 작으면</mark>** '오른쪽의 숫자 - 왼쪽 숫자'를 진행하면 된다.

## 생각
**1. 일단 문자들을 어떤 식으로 받아야할 것인가?**
   - 각 문자열 하나당 하나의 값들이 들어있다. unordered_map을 사용하여 문자와 값(key-value)를 연결시켜주면 될 것 이다.

**2. 계산 방식**
- 여러개의 문자들로 이어진 배열로 Input을 받을 것이다. for문을 돌려 i번째와 i + 1 를 비교하여 계산 방식을 지정해주면 될 것이다.

## 구현
```cpp
class Solution {
public:
    int romanToInt(string s) {
        int result = 0;
        unordered_map<char, int> roman {
            {'I', 1},
            {'V', 5},
            {'X', 10},
            {'L', 50},
            {'C', 100},
            {'D', 500},
            {'M', 1000},
        };

        for(int i = 0; i < s.size() - 1; i++)
        {
            if(roman[s[i]] < roman[s[i + 1]])
            {
                result -= roman[s[i]];
            }
            else
            {
                result += roman[s[i]];
            }
        }

        return result + roman[s[s.size() - 1]];
    }
};
```

## 코드 관찰
- 일단 unordered_map을 사용해서 문제에서 주어진 문자열(key)와 문자열이 가지는 값(value)를 설정해준다.
- for문을 작성해주는데 **<mark>for문의 조건중 s.size()에 -1 을 해줘야한다.</mark>** 만약 'III'를 Input으로 받았으면 s.size()의 값은 3이다. 만약 여기서 s.size()에 '- 1' 을 해주지 않는다면 if의 조건 roman[s[i + 1]] 에서 마지막 인덱스 s[2]를 초과한 s[3]을 참조하게 되는 상황이 발생한다. 그렇기 때문에 s.size에 '- 1'을 해줘야한다.(물론 i < s.size(); 를 사용하여 if문을 만들수 있다.)
- if문 조건 작성시 **roman[s[i]] < roman[s[i + 1]]를 먼저 해줘야한다.** 만약 조건을 **roman[s[i]] > roman[s[i + 1]]** 을 먼저 적는다면 마지막 문자에서 roman[s[i + 1]]을 접근할 때 배열을 뛰어넘는 오류가 발생하기 때문이다.
- 그리고 마지막 return에서는 roman 배열의 마지막 인덱스는 따로 계산을 해줘야한다.

