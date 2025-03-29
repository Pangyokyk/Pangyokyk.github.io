---
layout: post
title: 207. Course Schedule
tags: [Leetcode, Coding Test, graph, topological sorting, medium]
date: 2025-03-29 +0800
math: true
toc : true
---



# 207. Course Schedule


## 문제

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return true if you can finish all courses. Otherwise, return false.

[문제 사이트](https://leetcode.com/problems/course-schedule/description/?envType=study-plan-v2&envId=top-interview-150) 

Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.


Example 2:

Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
 

Constraints:

- 1 <= numCourses <= 2000
- 0 <= prerequisites.length <= 5000
- prerequisites[i].length == 2
- 0 <= ai, bi < numCourses
- All the pairs prerequisites[i] are unique.



## 문제 해석

- 전형적인 위상 정렬 중 선수 과목에 관한 문제이다.
- 이 같은 위상 정렬 문제의 핵심은 Example 2 에서 볼 수 있는 **사이클** 이 존재하면 **위상 정렬을 할 수 없다** 라는 지식이 있어야한다.




## 구현(BFS)


```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> degree(numCourses, 0);
        vector<vector<int>> adj(numCourses, vector<int>());

        for(auto& p : prerequisites)
        {
            adj[p[1]].push_back(p[0]);
            degree[p[0]]++;
        }
        queue<int> q;
        for(int i = 0; i < numCourses; i++)
        {
            if(degree[i] == 0)
            {
                q.push(i);
            }
        }
        while(!q.empty())
        {
            int curr = q.front();
            q.pop();
            numCourses--;

            for(auto next : adj[curr])
            {
                if(--degree[next] == 0)
                {
                    q.push(next);
                }
            }
        }
        return numCourses == 0;
    }
};
```



## 코드 해설

- 위상 정렬 문제를 풀 때는 여러 방법이 있지만 **Kahn 알고리즘** 방식을 알면 쉽게 해결이 가능하다.
- [Kahn 알고리즘](https://pangyokyk.github.io/2025/03/29/Kahn-algorithm/) 은 위상 정렬 문제에 쓰이는 알고리즘으로 **진입 차수(degree) 와 BFS(큐) 방식을 사용하는 알고리즘 이다.**



```cpp
vector<int> degree(numCourses, 0);
vector<vector<int>> adj(numCourses, vector<int>());
// ex) adj = [ [], [] ] 이렇게 생겼다.
```

- degree는 진입 차수를 저장하기 위한 벡터로 numCourses(들어야 하는 과목) 만큼 만들어둔다.
- adj 는 과목마다 연결되어있는 노드를 저장하기 위한 벡터로 numsCourses 만큼 만들어두고 각 칸마다 vector<int>() 로 초기화한다.(**이는 위상정렬에서 노드를 만들때 자주 사용하는 방식이므로 꼭 익혀두자**)


```cpp
for(auto p : prerequisites)
{
    adj[p[1]].push_back(p[0]);
    degree[p[0]]++;
}
```

- p[1]은 **먼저 들어야하는 과목** 이고 p[0] 은 p[1] 을 **들어야 수강할 수 있는 과목** 이다.
- Example 1) prerequisites = [ [1, 0] ] 이다. '0' 과목을 들어야 '1' 과목을 들을 수 있기 때문에 '0' -> '1' 이런 방향 그래프로 나타낼 수 있다.
- '0'은 선수 과목없이 바로 들을 수 있는 과목이므로 진입 차수는 **'0'** 이라고 할 수 있고 '1'은 '0'의 선수 과목이 있기 때문에 진입 차수는 **'1'** 이라고 할 수 있다.




```cpp
queue<int> q;
for(int i = 0; i < numCourses; i++)
{
    if(degree[i] == 0)
    {
        q.push(i);
    }
}
```

- BFS 방식으로 풀 것이기 때문에 큐를 선언해준다.
- 진입 차수들이 저장되어 있는 degree에서 언제든 들을 수 있는 진입 차수 0 들을 q에 먼저 넣어준다.


```cpp
for(auto next : adj[curr])
{
    if(--degree[next] == 0)
    {
        q.push(next);
    }
}
```

- adj[curr] 은 진입차수가 0인 과목의 선수 과목인(진입차수가 '1' 이상) 다음 과목들이 들어있다.
- 전 과목은 수강했으므로 이제 연관된 다음 선수 과목의 진입차수는 당연히 '1'이 감소할 것이다. 
- 만약 감소한 진입 차수가 0 이라면 다음으로 들어야할 과목이므로 이를 큐에 넣어 다시 똑같이 반복하면 된다.



```cpp
return numCourses == 0;
```

- 위상 정렬의 핵심은 **사이클이 존재해서는 안된다** 라는 것이다.
- 만약 큐가 비게된다면 사이클이 존재하는 것이므로 numCourses 는 0이 될 수 없을 것이다.