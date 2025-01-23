---
layout: post
title: 58. Length of Last Word
tags: [Leetcode, Coding Test, string, easy]
date: 2025-01-23 +0800
math: true
toc : true
---



# 문제
Given a string s consisting of words and spaces, return the length of the last word in the string.

A word is a maximal substring consisting of non-space characters only.

 

Example 1:
Input: s = "Hello World"
Output: 5
Explanation: The last word is "World" with length 5.

Example 2:
Input: s = "   fly me   to   the moon  "
Output: 4
Explanation: The last word is "moon" with length 4.

Example 3:
Input: s = "luffy is still joyboy"
Output: 6
Explanation: The last word is "joyboy" with length 6.
 

Constraints:

1 <= s.length <= 10^4
s consists of only English letters and spaces ' '.
There will be at least one word in s.


****


## 문제 해석
string s 가 입력되면 그것의 맨 끝 단어의 길이를 출력하는 문제이다. 단 마지막에 **Example2** 의 경우처럼 공백이 존재할 수 있다.


****


## 생각
문자열의 끝을 알아내야함 > 공백 or 문자이다 > 공백일 경우 처음 문자열을 만날때까지 이동 후 그 다음 공백까지 문자 수 세기 > 문자일 경우 > 다음 공백 만날 때까지 문자수 세기


****


## 구현

```cpp
class Solution {
public:
    int lengthOfLastWord(string s) {
        int Count = 0;
        int slength = s.length() - 1;
        bool end = false;

        for(int i = slength; i >= 0; --i)
        {
            if(s[i] != ' ')
            {
                Count++;
                end = true;
            }
            else if(end)
            {
                break;
            }
        }
        return Count;
    }
};
```


****


## 코드 해석

```cpp
if(s[i] != ' ')
    {
        Count++;
        end = true;
    }
    else if(end)
    {
        break;
    }
```

- for문이 **s** 의 뒤에서부터 조사해나간다.
- 만약 공백이 아니라면 Count를 증가시켜주고 'end = true' 통해 다음 공백을 만나면 종료시킬 준비를 해준다.
- **else if(end) 는 조건이 작성되어있지 않다면** end = true일 때 실행된다.


****


# 후기 
솔직히 이 문제도 그냥 기초 물어보는 easy중의 easy 문제이다. 하지만 막상 생각한 것을 코드로 짜볼려니 아직 간간히 막히는 구간이 있다. 좀 더 사고력과 모르는 메소드가 나올 때마다 메모해 공부해 나가야겠다.