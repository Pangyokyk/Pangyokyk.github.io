---
layout: post
title: 151. Reverse Words in a String
tags: [Leetcode, Coding Test, string, two pointers, medium]
date: 2025-03-01 +0800
math: true
toc : true
---



# 151. Reverse Words in a String


****


## 문제
Given an input string s, reverse the order of the words.

A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

[문제 사이트](https://leetcode.com/problems/reverse-words-in-a-string/description/?envType=study-plan-v2&envId=top-interview-150) 

Example 1:

Input: s = "the sky is blue"
Output: "blue is sky the"

Example 2:

Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.

Example 3:

Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
 

Constraints:

- 1 <= s.length <= 10^4
- s contains English letters (upper-case and lower-case), digits, and spaces ' '.
- There is at least one word in s.
 

**Follow-up: If the string data type is mutable in your language, can you solve it in-place with O(1) extra space?**



****



## 문제 해석
- 문장 string 을 단어를 기준으로 반대로 출력하는 문제
- 입력의 string 은 공백이 자유분방하게 있지만 출력의 string은 띄어쓰기를 준수해야한다.
- string은 소문자로만 이루어져있다.
- **Follow-up 문제로 추가 공간을 할당하지 않고 O(1)의 공간복잡도로 문제를 풀어야한다.**


****


## 구현

```cpp
class Solution {
public:
    string reverseWords(string s) {
        reverse(s.begin(), s.end());

        int l = 0;
        int r = 0;
        int i = 0;
        int n = s.size();
        int lastword = 0;

        while(l < n)
        {
            while(l < n && s[l] == ' ')
            {
                l++;
            }
            int startword = i;

            while(l < n && s[l] != ' ')
            {
                s[i++] = s[l++];
                lastword = i;
            }
            reverse(s.begin() + startword, s.begin() + lastword);
            s[i++] = ' ';
        }
        s.resize(lastword);
        return s;
    }
};
```