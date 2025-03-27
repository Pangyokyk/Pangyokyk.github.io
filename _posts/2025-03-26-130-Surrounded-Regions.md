---
layout: post
title: 130. Surrounded Regions
tags: [Leetcode, Coding Test, graph, medium]
date: 2025-03-26 +0800
math: true
toc : true
---



# 130. Surrounded Regions



## 문제

You are given an m x n matrix board containing letters 'X' and 'O', capture regions that are surrounded:

- Connect: A cell is connected to adjacent cells horizontally or vertically.
- Region: To form a region connect every 'O' cell.
- Surround: The region is surrounded with 'X' cells if you can connect the region with 'X' cells and none of the region cells are on the edge of the board.


To capture a surrounded region, replace all 'O's with 'X's in-place within the original board. You do not need to return anything.

 

Example 1:

Input: board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]

Output: [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

Explanation:
![그림](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)


In the above diagram, the bottom region is not captured because it is on the edge of the board and cannot be surrounded.


Example 2:

Input: board = [["X"]]

Output: [["X"]]

 

Constraints:

- m == board.length
- n == board[i].length
- 1 <= m, n <= 200
- board[i][j] is 'X' or 'O'.




## 문제 해설

- 2차원 배열 board가 주어지면 'X' 로 둘러쌓인 'O'를 'X'로 변경하는 코드를 작성하는 문제
- **In the above diagram, the bottom region is not captured because it is on the edge of the board and cannot be surrounded.**(위의 다이어그램에서 **<mark>아래쪽 영역은 보드의 가장자리에 있어서 둘러쌀 수 없기 때문에 캡처되지 않았습니다.</mark>**)
- 이 문제의 핵심 아이디어는 **<mark>가장 바깥쪽 변의 'O'와 연결되어 있다면 절대 'X'로 둘러쌓일 수 없다</mark>** 라는 것이다.




## 구현(DFS-재귀)

```cpp
class Solution {
public:
    void DFS(vector<vector<char>>& board, int i, int j, int row, int col) {
        if(board[i][j] == 'O')
        {
            board[i][j] = 'P';

            if(i >= 1)  DFS(board, i-1, j, row, col);
            if(j >= 1)  DFS(board, i, j-1, row, col);
            if(i+1 < row)   DFS(board, i+1, j, row, col);
            if(j+1 < col)   DFS(board, i, j+1, row, col);
        }
    }
    void solve(vector<vector<char>>& board) {
        int row = board.size();
        int col = board[0].size();

        for(int i = 0; i < row; i++)
        {
            DFS(board, i, 0, row, col);
            if(col > 1)
            {
                DFS(board, i, col-1, row, col);
            }
        }

        for(int j = 1; j < col - 1; j++)
        {
            DFS(board, 0, j, row, col);
            if(row > 1)
            {
                DFS(board, row-1, j, row, col);
            }
        }

        for(int i = 0; i < row; i++)
        {
            for(int j = 0; j < col; j++)
            {
                if(board[i][j] == 'O')
                {
                    board[i][j] = 'X';
                }
                else if(board[i][j] == 'P')
                {
                    board[i][j] = 'O';
                }
            }
        }
    }
};
```

## 코드 해설

- 만약 이 문제가 면접에서 나왔다면 **DFS-재귀** 풀이 방법은 옳다고 볼 수 없다.
- board의 크기가 250x250 이라면 최악의 경우 **스택 오버플로우** 가 발생하기 때문이다.
- 그래도 DFS, BFS 문제는 두 가지 풀이를 모두 공부하는 것이 좋다.
- 코드 해설 전 이 문제는 **가장 바깥쪽 모서리의 'O'와 연결되어 있다면 절대 'X'로 둘러쌓일 수 없다** 라는 걸 알아야한다.


```cpp
void DFS(vector<vector<char>>& board, int i, int j, int row, int col)
```
- 함수 DFS는 **DFS-재귀** 방법으로 'O'와 연결되어 있는 다른 'O'를 4방향으로 찾는 함수이다.



```cpp
if(board[i][j] == 'O')
{
    board[i][j] = 'P';

    if(i >= 1)  DFS(board, i-1, j, row, col); // 왼쪽 탐색
    if(j >= 1)  DFS(board, i, j-1, row, col); // 위쪽 탐색
    if(i+1 < row)   DFS(board, i+1, j, row, col); // 오른쪽 탐색
    if(j+1 < col)   DFS(board, i, j+1, row, col); // 아래쪽 탐색
}
```

- 만약 'O'를 찾았다면 절대 바뀌지 않는 값이니 'P'로 구분해준다.
- 재귀를 활용해 4방향 탐색을 해준다.



```cpp
for(int i = 0; i < row; i++)
{
    DFS(board, i, 0, row, col); // 가장 왼쪽 변 탐색
    if(col > 1)
    {
        DFS(board, i, col-1, row, col); // 가장 오른쪽 변 탐색
    }
}
```

- 반복문을 통해 가장 모서리의 'O'를 찾아 DFS('O'와 이어져있는 다른 'O' 탐색 함수)를 실행해준다.


```cpp
for(int j = 1; j < col - 1; j++)
{
    DFS(board, 0, j, row, col); // 가장 위쪽 변 탐색
    if(row > 1)
    {
        DFS(board, row-1, j, row, col); // 가장 아래쪽 변 탐색
    }
}
```

- 동일하다.



```cpp
for(int i = 0; i < row; i++)
{
    for(int j = 0; j < col; j++)
    {
        if(board[i][j] == 'O')
        {
            board[i][j] = 'X';
        }
        else if(board[i][j] == 'P')
        {
            board[i][j] = 'O';
        }
    }
}
```
- 이제 'P'로 바꾸지 않은 나머지 'O'는 전부 'X'로 바꿔준다.
- 바꿧던 'P'를 'O'로 돌려주면 끝이다.





## 구현(BFS)


```cpp
class Solution {
public:
    void BFS(vector<vector<char>>& board, int i, int j) {
        queue<pair<int, int>> q;

        board[i][j] = 'P';

        q.push(make_pair(i, j));
        int m = board.size();
        int n = board[0].size();

        while(!q.empty())
        {
            auto next = q.front();
            int x = next.first;
            int y = next.second;
            q.pop();

            board[x][y] = 'P';
        
            if(x >= 1 && board[x-1][y] == 'O')
            {
                board[x-1][y] = 'P';
                q.push(make_pair(x-1, y));
            }
            if(x+1 < m && board[x+1][y] == 'O' )
            {
                board[x+1][y] = 'P';
                q.push(make_pair(x+1, y));
            }
            if(y >= 1 && board[x][y-1] == 'O')
            {
                board[x][y-1] = 'P';
                q.push(make_pair(x, y-1));
            }
            if(y+1 < n && board[x][y+1] == 'O')
            {
                board[x][y+1] = 'P';
                q.push(make_pair(x, y+1));
            }
        }
    }

    void change(vector<vector<char>>& board) {
        int m = board.size();
        int n = board[0].size();

        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(board[i][j] == 'O')
                {
                    board[i][j] = 'X';
                }
                else if(board[i][j] == 'P')
                {
                    board[i][j] = 'O';
                }
            }
        }
    }

    void solve(vector<vector<char>>& board) {
        int m = board.size();
        int n = board[0].size();

        for(int i = 0; i < m; i++)
        {
            if(board[i][0] == 'O')
            {
                BFS(board, i, 0);
            }
            if(board[i][n-1] == 'O')
            {
                BFS(board, i, n-1);
            }
        }

        for(int i = 1; i < n - 1; i++)
        {
            if(board[0][i] == 'O')
            {
                BFS(board, 0, i);
            }
            if(board[m-1][i] == 'O')
            {
                BFS(board, m-1, i);
            }
        }
        change(board);
    }
};
```



## 코드해설

- **DFS-반복**과 유사한 코드이다.
- BFS는 queue를 사용한다는 점이 다르다.



## 🎯 면접 질문


### **<mark>왜 해당 문제를 DFS 방식 중 스택으로 푸는 것이 가장 좋은가?</mark>**
[해당 문제와 같은 맥락의 문제](https://pangyokyk.github.io/2025/03/25/200-Number-of-Islands/)


- **<mark>가장 핵심적인 것은 재귀를 사용했을 때 최악의 조건이 주어졌을 경우 스택 오버플로우가 발생할 수 있다는 점을 알아야한다.</mark>**

