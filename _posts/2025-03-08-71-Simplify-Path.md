---
layout: post
title: 71. Simplify Path
tags: [Leetcode, Coding Test, string, stack, medium]
date: 2025-03-08 +0800
math: true
toc : true
---


# 71. Simplify Path


****


## 문제

You are given an absolute path for a Unix-style file system, which always begins with a slash '/'. Your task is to transform this absolute path into its simplified canonical path.

The rules of a Unix-style file system are as follows:

- A single period '.' represents the current directory.
- A double period '..' represents the previous/parent directory
- Multiple consecutive slashes such as '//' and '///' are treated as a single slash '/'.
- Any sequence of periods that does not match the rules above should be treated as a valid directory or file name. For example, '...' and '....' are valid directory or file names.

The simplified canonical path should follow these rules:
- The path must start with a single slash '/'.
- Directories within the path must be separated by exactly one slash '/'.
- The path must not end with a slash '/', unless it is the root directory.
- The path must not have any single or double periods ('.' and '..') used to denote current or parent directories.
 
Return the simplified canonical path.

[문제 사이트](https://leetcode.com/problems/simplify-path/description/?envType=study-plan-v2&envId=top-interview-150) 

Example 1:

Input: path = "/home/"

Output: "/home"

Explanation:

The trailing slash should be removed.

Example 2:

Input: path = "/home//foo/"

Output: "/home/foo"

Explanation:

Multiple consecutive slashes are replaced by a single one.

Example 3:

Input: path = "/home/user/Documents/../Pictures"

Output: "/home/user/Pictures"

Explanation:

A double period ".." refers to the directory up a level (the parent directory).

Example 4:

Input: path = "/../"

Output: "/"

Explanation:

Going one level up from the root directory is not possible.

Example 5:

Input: path = "/.../a/../b/c/../d/./"

Output: "/.../b/d"

Explanation:

"..." is a valid name for a directory in this problem.

 

Constraints:

- 1 <= path.length <= 3000
- path consists of English letters, digits, period '.', slash '/' or '_'.
- path is a valid absolute Unix path.



****


## 문제 해석

- 파일 경로 호출의 결과를 출력하는 문제
- 문제에 조건을 잘 해석해야한다.
- '/' 는 하나만 존재 가능, 연속된 '/'는 무시한다.
- '.' 은 현재 디렉토리, '..'은 부모 디렉토리, '...' 이상은 그러한 이름의 파일이 있다고 생각




****


## 구현


```cpp
class Solution {
public:
    string simplifyPath(string path) {
        stack<string> stk;
        string ans;

        for(int i = 0; i < path.size(); i++)
        {
            if(path[i] == '/')
            {
                continue;
            }

            string prev;
            while(i < path.size() && path[i] != '/')
            {
                prev += path[i];
                i++;
            }

            if(prev == ".")
            {
                continue;
            }
            else if(prev == "..")
            {
                if(!stk.empty())
                {
                    stk.pop();
                }
            }
            else
            {
                stk.push(prev);
            }
        }
        while(!stk.empty())
        {
            ans = "/" + stk.top() + ans;
            stk.pop();
        }
        if(ans.size() == 0)
        {
            return "/";
        }
        return ans;
    }
};
```



****


## 코드 해설

```cpp
if(path[i] == '/')
{
    continue;
}
```

- '/'가 나오면 다음 반복으로 넘어간다.


```cpp
string prev;
while(i < path.size() && path[i] != '/')
{
    prev += path[i];
    i++;
}
```

- 만약 '/' 가 아니라면 그것을 string prev 로 저장해 모아둔다.
- 이제 모아둔 prev의 조건을 따져봐야한다.


```cpp
if(prev == ".")
{
    continue;
}
```

- 만약 prev가 '.' 라면 스택에 넣지도 않고 무시하고 넘어간다.


```cpp
else if(prev == "..")
{
    if(!stk.empty())
    {
        stk.pop();
    }
}
```

- ".." 은 부모 디렉토리로 넘어가야한다.
- 그렇기 때문에 그동안 쌓은 stack stk 에서 맨 위를 삭제해주면된다.
- **".."은 맨 처음에 올 수도 있다. 그때는 스택이 비어있어 stk.pop()을 하면 오류가 발생하기 때문에 꼭 !stk.empty() 조건이 필요하다.**


```cpp
else
{
    stk.push(prev);
}
```

- 위 조건이 모두 아니라면 string prev는 파일 이름으로 stk에 넣어주면 된다.



```cpp
ans = "/" + stk.top() + ans;
```

- **<mark>이 문제에서 가장 핵심 코드</mark>**
- stack 은 선입후출(FILO) 의 형태이기 때문에 위 코드대로 하면 경로가 거꾸로 된 채로 출력될 것이다.

```cpp
stk({"home"}, {"sweet"}, {"man"});
```
- 만약 stk가 이러한 상태라고 하자

1. ans = "/" + "man" + ""; (-> ans = /man)
2. ans = "/" + "sweet" + "/man" (-> ans = /sweet/man)
3. ans = "/" + "home" + "/sweet/man" (->ans = /home/sweet/man)

- 이러한 과정이 거치게 된다.