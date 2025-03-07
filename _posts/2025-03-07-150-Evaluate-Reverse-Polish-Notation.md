---
layout: post
title: 150. Evaluate Reverse Polish Notation
tags: [Leetcode, Coding Test, stack, hash table, medium]
date: 2025-03-07 +0800
math: true
toc : true
---


# 150. Evaluate Reverse Polish Notation


You are given an array of strings tokens that represents an arithmetic expression in a Reverse Polish Notation.

Evaluate the expression. Return an integer that represents the value of the expression.

Note that:

- The valid operators are '+', '-', '*', and '/'.
- Each operand may be an integer or another expression.
- The division between two integers always truncates toward zero.
- There will not be any division by zero.
- The input represents a valid arithmetic expression in a reverse polish notation.
- The answer and all the intermediate calculations can be represented in a 32-bit integer.
 
[문제 사이트](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/?envType=study-plan-v2&envId=top-interview-150)

Example 1:

Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9

Example 2:

Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6

Example 3:

Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
 

Constraints:

1 <= tokens.length <= 10^4
tokens[i] is either an operator: "+", "-", "*", or "/", or an integer in the range [-200, 200].



****


## 문제 해석
- string 형태의 token이 **Reverse Polish Notation** 계산법으로 표현되어있는걸 계산하라는 문제



****


## 구현(self)

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> stk;
        int result = 0;
        
        for(auto& s : tokens)
        {
            if( s == "+")
            {
                int a = stk.top();
                stk.pop();
                int b = stk.top();
                stk.pop();
                stk.push(b+a);
            }
            else if( s == "-")
            {
                int a = stk.top();
                stk.pop();
                int b = stk.top();
                stk.pop();
                stk.push(b-a);
            }
            else if( s == "*")
            {
                int a = stk.top();
                stk.pop();
                int b = stk.top();
                stk.pop();
                stk.push(b*a);
            }
            else if( s == "/")
            {
                int a = stk.top();
                stk.pop();
                int b = stk.top();
                stk.pop();
                stk.push(b/a);
            }
            else
            {
                stk.push(stoi(s));
            }
        }
        return stk.top();
    }
};
```


****


## 코드 해설
- 단순하게 stack을 이용한 풀이
- 하지만 더 깔끔하게 표현할 수 있는 코드가 있다.




****


## 구현(정답)

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        unordered_map<string, function<int (int, int)>> map = {
            {"+", [] (int a, int b) {return a + b;}},
            {"-", [] (int a, int b) {return a - b;}},
            {"*", [] (int a, int b) {return a * b;}},
            {"/", [] (int a, int b) {return a / b;}}
        };

        stack<int> stk;

        for(auto& a : tokens)
        {
            if(!map.count(a))
            {
                stk.push(stoi(a));
            }
            else
            {
                int top1 = stk.top();
                stk.pop();
                int top2 = stk.top();
                stk.pop();
                stk.push(map[a](top2, top1));
            }
        }
        return stk.top();
    }
};
```


****


## 코드 해설

```cpp
unordered_map<string, function<int (int, int)>> map
```
- **핵심 아이디어**
- hash table 에 int, char, string 만이 아니라 **함수** 도 집어 넣을 수 있다.
- 맨 앞 int 는 반환타입
- (int, int) 는 들어 오는 인자이다.


```cpp
unordered_map<string, function<int (int, int)>> map = {
    {"+", [] (int a, int b) {return a + b;}},
    {"-", [] (int a, int b) {return a - b;}},
    {"*", [] (int a, int b) {return a * b;}},
    {"/", [] (int a, int b) {return a / b;}}
};
```

- **주의 깊게 봐야할 것은 'fuction을 어떻게 선언했는가?'이다**
- <mark>[ ]</mark> 는 **람다 표현식** 으로 익명 함수를 만드는 방법이다.

```cpp
[캡처](매개변수) -> 반환타입 { 함수 본문 }
```
- 람다 표현식의 기본 형태이다.
- (int a, int b) -> 매개변수
- {return a + b} -> 함수 본문

```cpp
map[a](top2, top1) // 문제에서 람다 표현식 사용
```

- 또한 이번 문제를 풀면서 알게된 것은 count() 메소드 이다.


```cpp
if(!map.count(a))
```


- hash table 에서 count()는 존재한다면 '1' 존재하지 않는다면 '0' 을 반환한다.
- **'0' = false, '0' 을 제외한 나머지 숫자 = true 라고 인식한다.**
- 즉 map.count(a) == 0 (hash table에 존재하지 않는다면, 여기서는 숫자라면) !0(=!false) -> true 가 되므로 숫자라면 if문이 실행되는 것이다.