---
layout: post
title: 841. Keys and Rooms
tags: [Leetcode, Coding Test, dfs, bfs, medium]
date: 2025-02-21 +0800
math: true
toc : true
---



# 841. Keys and Rooms



****


## 문제

There are n rooms labeled from 0 to n - 1 and all the rooms are locked except for room 0. Your goal is to visit all the rooms. However, you cannot enter a locked room without having its key.

When you visit a room, you may find a set of distinct keys in it. Each key has a number on it, denoting which room it unlocks, and you can take all of them with you to unlock the other rooms.

Given an array rooms where roomㄴs[i] is the set of keys that you can obtain if you visited room i, return true if you can visit all the rooms, or false otherwise.

[문제 사이트](https://leetcode.com/problems/keys-and-rooms/description/?envType=study-plan-v2&envId=leetcode-75) 

Example 1:

Input: rooms = [[1],[2],[3],[]]
Output: true
Explanation: 
We visit room 0 and pick up key 1.
We then visit room 1 and pick up key 2.
We then visit room 2 and pick up key 3.
We then visit room 3.
Since we were able to visit every room, we return true.


Example 2:

Input: rooms = [[1,3],[3,0,1],[2],[0]]
Output: false
Explanation: We can not enter room number 2 since the only key that unlocks it is in that room.
 

Constraints:
- n == rooms.length
- 2 <= n <= 1000
- 0 <= rooms[i].length <= 1000
- 1 <= sum(rooms[i].length) <= 3000
- 0 <= rooms[i][j] < n
- All the values of rooms[i] are unique.



****


## 문제 해석
- 각 방에 키가 존재하는데 모든 방을 다 들어갈 수 있는지 묻는 문제
- 첫번째 방 rooms[0] 부터 시작한다.



****


## 구현(dfs)

```cpp
class Solution {
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        vector<bool> visited(rooms.size(), false);  
        dfs(rooms, visited, 0);

        for(auto a : visited)
        {
            if(a == false)  return false;
        } 
        return true;
    }

    void dfs(vector<vector<int>>& rooms, vector<bool>& visited, int room)   {
        visited[room] = true;
        for(auto key : rooms[room])
        {
            if(visited[key] == false)
            {
                dfs(rooms, visited, key);
            }
        }
    }
};
```



****


## 코드 해석

```cpp
visited[room] = true;
```

- 재귀적으로 호출할 것이기 때문에 다시 호출했다라는 것은 그 방을 방문했다라는 뜻이다. 그렇기 때문에 true 를 호출 때마다 해준다.


```cpp
for(auto key : rooms[room])
{
    if(visited[key] == false)
    {
        dfs(rooms, visited, key);
    }
}
```
- rooms[room] 에는 방문을 열 수 있는 키 vector<int> 들이 들어있다.
- visited 는 rooms의 크기와 동일하고 전부 false로 채워져있는 벡터이다.
- 이것이 만약 false라면 다시 재귀적으로 호출하여 모든 방과 key에 대해 진행한다.




****


## 구현(bfs)

```cpp
class Solution {
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        vector<bool> visited(rooms.size(), false);
        queue<int> q;
        visited[0] = true;
        q.push(0);
        while(!q.empty())
        {
            int key = q.front();
            q.pop();
            
            for(auto a : rooms[key])
            {
                if(visited[a] == false)
                {
                    visited[a] = true;
                    q.push(a);
                }
            }
        }

        for(auto a : visited)
        {
            if(a == false)  return false;
        }
        return true;
    }
};
```

- **bfs는 queue를 활용하는 풀이법이다.**