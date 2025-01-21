---
layout: post
title: Valid Parentheses
tags: [Leetcode, Coding Test, stack]
date: 2025-01-21 +0800
math: true
toc : true
---

# Leetcode : Valid Parentheses

## 문제

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

****

## 문제 해석
string으로 6종류의 괄호가 Input으로 들어온다. 우리가 수학적으로 생각하는 괄호가 잘 닫혀있다고 생각되면 리턴 값이 **true** 가 되고 잘 닫혀있지 않으면 **false** 가 된다. 

****

## 생각
- 일단 '(', ')' ㅡ '[', ']' ㅡ '{', '}' 은 같은 value로 지정하여 양 옆을 반복해서 조사해 value값이 같으면 true가 되게 하면 된다
- 순서 상관없이 (key - value)를 지정해주면 되니 unordered_map을 사용하면 될 것이다.

****

## 구현(1) 

```cpp
class Solution {
public:
    bool isValid(string s) {
        unordered_map<char, int> Parent = {
            {'(', 0 },
            {')', 0 },
            {'[', 1 },
            {']', 1 },
            {'{', 2 },
            {'}', 2 }
        };
        
        if(s.size() % 2 == 1)
        {
            return false;
        }
        
        for(int i = 0; i < s.size() / 2; i++)
        {
            if(Parent[s[i]] != Parent[s[s.size() - i - 1]])
            {
                return false;
            }
        }
        return true;
    }
};
```

****

## 오류
```cpp
s = "()"
```

```cpp
s = "(]"
```
- 위 두 경우에는 정답이 true 와 false로 잘 나왔다 하지만 내가 생각을 하지 못한 경우가 있었다. 

```cpp
s = "()[]{}"
```
- 내가 생각하고 짜낸 코드는 <mark>왼쪽 끝</mark> 과 <mark>오른쪽 끝</mark> 을 검사해 안으로 좁혀가는 식으로 생각했는데 위의 경우도 괄호가 잘 닫혀 **true** 값이 도출되야 하지만 내가 짜낸 코드에서는 왼쪽 끝과 오른쪽 끝이 같지 않기 때문에 **false** 라는 값이 도출된다
- **그렇기 때문에 이러한 방식이 아니라 <mark>스택을 사용해서 검사하는 방식</mark> 을 택해야한다.**

****

## 구현(2)

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> stk;
        unordered_map<char, char> mapping {
            {')', '('},
            {']', '['},
            {'}', '{'}
        };

        for(char c : s)
        {
            if(mapping.find(c) == mapping.end()) 
            {
                stk.push(c);
            }
            else if(!stk.empty() && mapping[c] == stk.top())
            {
                stk.pop();
            }
            else
            {
                return false;
            }
        }

        return stk.empty();

    }
};
```

****

## 코드 관찰
1. 먼저 unordered_map 의 key 값은 **닫는 괄호** 이여야한다.
   - 처음 스택에 넣어지는 괄호는 **여는 괄호** 이기에 key-value를 비교하기 위해서는 **닫는 괄호** 가 key값이여야 한다.

2. empty()는 비어있다면 true를 리턴하고 비어있지 않다면 false를 리턴한다.

****

## 후기
이제 답을 보고 공부하는 방식에서 내가 한번 끝까지 생각하고 코드를 짜는 식으로 해보았다. 느낀 바로는 아직 내가 <mark>컴퓨팅적 사고회로</mark> 가 부족한 것 같다. 먼저 문제를 이해하기 위해 수학적으로 접근한 다음 이걸 컴퓨터에게 이해를 시킬려면 어떻게 해야할까를 고민하는 순서를 더 공부해야겠다.
