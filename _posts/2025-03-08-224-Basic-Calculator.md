---
layout: post
title: 224. Basic Calculator
tags: [Leetcode, Coding Test, string, stack, hard]
date: 2025-03-08 +0800
math: true
toc : true
---


# 224. Basic Calculator


****


## 문제

Given a string s representing a valid expression, implement a basic calculator to evaluate it, and return the result of the evaluation.

**Note: You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as eval().**

[문제 사이트](https://leetcode.com/problems/basic-calculator/description/?envType=study-plan-v2&envId=top-interview-150)

Example 1:

Input: s = "1 + 1"
Output: 2

Example 2:

Input: s = " 2-1 + 2 "
Output: 3

Example 3:

Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
 

Constraints:

- 1 <= s.length <= 3 * 10^5
- s consists of digits, '+', '-', '(', ')', and ' '.
- s represents a valid expression.
- '+' is not used as a unary operation (i.e., "+1" and "+(2 + 3)" is invalid).
- '-' could be used as a unary operation (i.e., "-1" and "-(2 + 3)" is valid).
- There will be no two consecutive operators in the input.
- Every number and running calculation will fit in a signed 32-bit integer.



****


## 문제 해석

- string s 로 계산식이 주어지면 계산의 결과 값을 도출해내는 문제
- **문자를 숫자로 바꿔주는 메소드를 쓰면 안된다. ex)stoi()**



****


## 구현


```cpp
class Solution {
public:
    int calculate(string s) {
        int total = 0; // 계산결과 저장
        int num = 0; //현재 들어온 숫자 저장
        stack<int> stk({1}); //괄호 계산에 쓰일 스택
        int plusminus = 1; //부호 계산에 쓰일 값

        for(char& c : s)
        {
            if(isdigit(c))
            {
                num = num * 10 + (c - '0');
            }
            else
            {
                total += num * plusminus * stk.top();
                num = 0;
                if(c == '+')
                {
                    plusminus = 1;
                }
                if(c == '-')
                {
                    plusminus = -1;
                }
                if(c == '(')
                {
                    stk.push(plusminus * stk.top());
                    plusminus = 1;
                }
                if(c == ')')
                {
                    stk.pop();
                }
            }
        }
        total += plusminus * num * stk.top();
        return total;
    }
};
```



****



## 코드 해설

- 난이도가 **Hard** 인 만큼 구현 아이디어 떠오르는 것이 매우 까다롭다.
- 문제에서 문자열을 수학적 표현식으로 평가하는 내장 함수는 쓰지 못하게 막았다. 만약 쓸 수 있다면 그냥 SUM 문제이니 당연하다.
- 이 문제를 푸는 아이디어는 **스택의 사용처** 이다.


```cpp
stack<int> stk({1});
```
- int형 스택을 선언하는데 **'1' 을 미리 넣어둔다.**


```cpp
if(isdigit(c))
```
- isdigit() 함수는 char이 '0 ~ 9' 범위의 정수인지 true, false로 판별해주는 내장 함수이다.


```cpp
num = num * 10 + (c - '0');
```
- '0' 는 char로 ASCII 로 48의 값을 가지고 있다.
- 즉 c - '0' 을 거치면 int c 값이 나오게 되는 것이다.
- num * 10 을 해주는 이유는 십의자리 이상의 숫자를 계산하기 위함이다.
- **c - '0'에 괄호를 해주지 않으면 오버플로우가 발생한다.**



```cpp
total += num * plusminus * stk.top();
```

- else 부분 즉, 숫자가 아닌 기호가 들어왔을 때 경우를 살펴보자.
- 이 부분 또한 생각하기 어려운 부분이다.
- num 은 연산에 쓰일 숫자
- plusminus 는 num의 부호 상태, '+(1)' or '-(-1)' 을 저장하고 있다.
- stk는 괄호 연산의 부호를 가지고 있다. 
- 1 - (2 + 3) 의 형태라면 괄호 안의 2 + 3 은 괄호 앞 부호가 - 이므로 2 + 3의 결과 값을 빼줘야 하는 것이다.


```cpp
if(c == '(')
{
    stk.push(plusminus * stk.top());
    plusminus = 1;
}
```

- '(' 은 괄호가 시작되는 순간이다.
- **즉 괄호 뒤의 결과 값들은 전부 '(' 앞 부호에 따라 덧셈, 뺄셈이 결정 되기 때문에 stk.push(plusminus * stk.top()); 스택에 넣어 부호의 값을 저장해줘야 하는 것이다.**


```cpp
if(c == ')')
{
    stk.pop();
}
```

- 괄호가 끝나면 괄호 계산에 쓰였던 부호는 삭제해줘야한다.



```cpp
total += plusminus * num * stk.top();
```

- 맨 마지막 숫자를 계산해주기 위한 식
- 맨 마지막 숫자는 num에 머물러 있고 다음 부호가 나오지 않기 때문에 따로 계산을 해줘야한다.
