---
layout: post
title: 200. Number of Islands
tags: [Leetcode, Coding Test, array, dfs, bfs, union find, medium]
date: 2025-03-25 +0800
math: true
toc : true
---





# 200. Number of Islands



## 문제

Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

[문제 사이트](https://leetcode.com/problems/number-of-islands/description/?envType=study-plan-v2&envId=top-interview-150)

Example 1:

Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1

Example 2:

Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
 

Constraints:

- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 300
- grid[i][j] is '0' or '1'.




## 문제 해석

- '0'(물)을 경계로 독립된 섬의 개수를 출력하는 문제
- **4방향(위, 아래, 왼쪽, 오른쪽)** 만 이어져있다고 생각한다.





## 구현


```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int island = 0;

        stack<pair<int, int>> stk;

        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(grid[i][j] == '1')
                {
                    stk.push(make_pair(i, j));
                    island++;
                    while(!stk.empty())
                    {
                        int x = stk.top().first;
                        int y = stk.top().second;
                        stk.pop();

                        grid[x][y] = '0';

                        if(y + 1 < n && grid[x][y+1] == '1')
                        {
                            stk.push(make_pair(x, y+1));
                        }

                        if(x + 1 < m && grid[x+1][y] == '1')
                        {
                            stk.push(make_pair(x+1, y));
                        }

                        if(x - 1 >= 0 && grid[x-1][y] == '1')
                        {
                            stk.push(make_pair(x-1, y));
                        }

                        if(y - 1 >= 0 && grid[x][y-1] == '1')
                        {
                            stk.push(make_pair(x, y-1));
                        }
                    }
                }
            }
        }
        return island;
    }
};
```



## 코드 해설

```cpp
int m = grid.size();
int n = grid[0].size();
int island = 0;
```

- m 은 2차원 배열 grid 의 행의 개수를 나타내고 n은 열의 개수를 나타낸다.


```cpp
stack<pair<int, int>> stk;
```

- <int, int> 짝이 하나의 스택인 stk를 선언한다.(DFS stack방식으로 풀 것이기 때문)


```cpp
for(int i = 0; i < m; i++)
{
    for(int j = 0; j < n; j++)
    {

    }
}
```

- 2중 중첩 for문을 사용해 grid 의 '1'(섬)을 탐색할 것이다.



```cpp
if(grid[i][j] == '1')
{
    stk.push(make_pair(i, j));
    island++;
}
```

- 만약 for문을 반복하다가 '1'(섬)을 만나면 해당 위치를 스택에 넣고 섬의 개수를 증가시킨다.
- 이제 여기서 이 위치의 섬과 이어진 '1'(섬)은 하나의 같은 섬으로 취급하기 때문에 어디까지 이어져있는지 찾아야한다.



```cpp
int x = stk.top().first;
int y = stk.top().second;
```

- x 는 stack<pair<int, int>>의 첫번째 값, 즉 **행의 위치**
- y 는 stack<pair<int, int>>의 두번째 값, 즉 **열의 위치**



```cpp
grid[x][y] = '0';
```

- **탐색한 땅은 이미 방문해서 섬으로 취급했기 때문에 중복을 방지하기 위해 '0'으로 바꿔준다.**



```cpp
// 오른쪽 땅 탐색
if(y + 1 < n && grid[x][y+1] == '1')
{
    stk.push(make_pair(x, y+1));
}
//아래쪽 땅 탐색
if(x + 1 < m && grid[x+1][y] == '1')
{
    stk.push(make_pair(x+1, y));
}
//왼쪽 땅 탐색
if(x - 1 >= 0 && grid[x-1][y] == '1')
{
    stk.push(make_pair(x-1, y));
}
//위쪽 땅 탐색
if(y - 1 >= 0 && grid[x][y-1] == '1')
{
    stk.push(make_pair(x, y-1));
}
```


- 해당 문제에서 이어진 땅은 **4방향(위, 아래, 왼쪽, 오른쪽)** 만 취급한다고 했다.
- 각 처음 **x와 y의 범위 조건** 은 오버플로우가 발생하니 꼼꼼히 확인하자.





## 🎯 면접 질문

### **<mark>왜 해당 문제를 DFS 방식 중 스택으로 푸는 것이 가장 좋은가?</mark>**



**✅ 1. 스택 DFS는 메모리 사용량이 적다**
- BFS는 큐(queue)를 사용하므로, 최악의 경우 모든 노드를 저장해야 한다.
- 하지만 DFS(스택)은 필요할 때만 노드를 저장하므로 더 메모리 효율적이다.
- 큰 맵(1000x1000)에서는 BFS는 너무 많은 메모리를 사용할 가능성이 크다.


**✅ 2. DFS는 구현이 간결하다**
- DFS는 **재귀** 또는 **스택**으로 사용하여 직관적으로 구현 가능


**✅ 3. DFS는 최단 거리 계산이 필요 없는 문제에 적합**
- BFS는 최단 거리 문제에서 유용하지만, 이 문제는 섬의 개수 파악문제




### **<mark>DFS(재귀) 와 DFS(반복) 중 왜 반복을 사용해야 하는가?</mark>**

**✅ 재귀 DFS vs 스택 DFS**

|방식|메모리사용|스택 오버플로우 가능성|속도|
|---------|---------|-----------------------|----|
|**재귀 DFS**| 콜 스택 사용 | **스택 오버플로우 위험(큰 맵의 경우)** | 빠름|
|**반복 DFS**| 명시적 스택 사용| 스택 크기 조절 가능 | 빠름|

- **즉, 재귀 DFS를 사용하면 grid의 크기가 너무 클 경우 스택 오버플로우의 위험성이 있기 때문에 <mark>반복 DFS</mark> 로 푸는 방법이 가장 좋다고 할 수 있다.** 







