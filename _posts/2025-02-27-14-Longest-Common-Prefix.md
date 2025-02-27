---
layout: post
title: 14. Longest Common Prefix
tags: [Leetcode, Coding Test, string, sliding window, easy]
date: 2025-02-27 +0800
math: true
toc : true
---




# 14. Longest Common Prefix



****


## 문제

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

[문제 사이트](https://leetcode.com/problems/longest-common-prefix/description/?envType=study-plan-v2&envId=top-interview-150)

Example 1:

Input: strs = ["flower","flow","flight"]
Output: "fl"

Example 2:

Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
 

Constraints:

- 1 <= strs.length <= 200
- 0 <= strs[i].length <= 200
- strs[i] consists of only lowercase English letters if it is non-empty.


****



## 문제 해석
- 가장긴 공통된 접두사를 추출하는 문제
- strs는 모두 소문자로 이루어져 있다.



****


## 구현


```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.size() == 0)    return "";

        string pref = strs[0];
        int prefLen = pref.size();

        for(int i = 1; i < strs.size(); i++)
        {
            string s = strs[i];
            while(prefLen > s.size() || pref != s.substr(0, prefLen))
            {
                prefLen--;
                if(prefLen == 0)
                {
                    return "";
                }
                pref = pref.substr(0, prefLen);
            }
        }
        return pref;
    }
};
```



****


## 코드 해석

- **easy로 분류되어있지만 정답률은 44%인 medium 수준의 문제이다.**
- strs를 정렬하여 푸는 문제도 있지만 시간복잡도가 O(\(nlongn\)) 해당하기 때문에 적절치 않다.


```cpp
if(strs.size() == 0)    return "";
```
- 처음 기본 조건이다.
- strs가 비어있다면 공백을 출력한다.


```cpp
string pref = strs[0];
int prefLen = pref.size();
```
- **비교할 문자열을 strs[0], 첫번째로 고정한다.**

```cpp
for(int i = 1; i < strs.size(); i++)
```
- strs[0] 은 비교 대상으로 고정해놨으니 strs[1] 부터 시작해야 하므로 **i = 1** 시작이다.


```cpp
while(prefLen > s.size() || pref != s.substr(0, prefLen))
{
    prefLen--;
    if(prefLen == 0)
    {
        return "";
    }
    pref = pref.substr(0, prefLen);
}
```
- **이번 문제에서 핵심 아이디어이다.**
- while 문은 pref = strs[0] 을 문자열을 한개씩 지워가며 공통된 접두사들을 찾는 코드를 쓸 것이다.
- 즉, <mark>**Sliding Window**</mark> 기법을 사용하는 것이다.
- while 문의 조건문을 살펴보면

```cpp
prefLen > s.size() 
```
- prefLen이 s의 길이보다 더 크다면 공통된 접두사의 길이가 절대 될 수 없기 때문에 s.size()에 맞춰줘야한다.


```cpp
pref != s.substr(0, prefLen)
```
- pref 와 s 가 동일하지 않을 때 점점 비교하는 문자열을 줄여가야한다.



### std::string 에서의 substr

- string 에서 범위를 지정해서 그 부분만 읽어오고 싶을 떄 substr()을 사용한다.
- <mark>**substr(시작위치, 읽을 길이)**</mark>