---
layout: post
title: 28. Find the Index of the First Occurrence in a String
tags: [Leetcode, Coding Test, string, easy]
date: 2025-01-22 +0800
math: true
toc : true
---



# 문제
Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

Example 1:

Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.
Example 2:

Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.
 

Constraints:

1 <= haystack.length, needle.length <= 10^4
haystack and needle consist of only lowercase English characters.


****


## 문제 해석
두개의 문자열이 주어지고 needle과 동일한 문자열이 haystack에 존재하면 처음 시작되는 index를 리턴하고 없다면 -1을 리턴하면 된다.


****


## 생각
find를 이용해서 needle의 문자열을 검색 > 만약 찾으면 처음 시작인 인덱스 찾기 > 만약 같은 단어가 중복으로 있다면? > 더 작은 숫자를 리턴 > 존재하지 않는다면 '-1' 리턴


****


## 구현

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        size_t findstr = haystack.find(needle);

        if(findstr != std::string::npos) //찾음
        {
            return findstr;
        }
        else
        {
            return findstr; // 못찾으면 -1 값이 나옴
        }
    }
}
```


****


## 코드 해석
- size_t는 크기 또는 수의 양을 표현하는데 사용되는 부호 없는 정수 타입이다.
- 만약 find를 해서 사용하여 찾았다면 그 값은 처음 발견한 **index의 시작 번호를 반환한다** (즉, 위에 생각에서 했던 '중복이 존재한다면 더 작은 값을 리턴한다'를 생각하지 않아도 된다.)
- **npos** 는 C++ STL에서 미리 정의된 상수로 std::string의 멤버이다. 이 값은 std::string::find 메소드에서 검색에 실패했을 때 사용되는 **<mark>특별한 값</mark>** 으로 사용된다. 일반적으로 **찾지 못했다** 라는 의미이다.


****


## 후기
문제를 푸는데 사용되는 문법들을 정리해가며 자주 접해봐야 코테의 실력이 느는 것 같다.

