---
layout: post
title: 210. Course Schedule II
tags: [Leetcode, Coding Test, graph, topological sorting, medium]
date: 2025-03-29 +0800
math: true
toc : true
---


# 210. Course Schedule II



## 문제

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

[문제 사이트](https://leetcode.com/problems/course-schedule-ii/description/?envType=study-plan-v2&envId=top-interview-150) 

Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].


Example 2:

Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].


Example 3:

Input: numCourses = 1, prerequisites = []
Output: [0]
 

Constraints:

- 1 <= numCourses <= 2000
- 0 <= prerequisites.length <= numCourses * (numCourses - 1)
- prerequisites[i].length == 2
- 0 <= ai, bi < numCourses
- ai != bi
- All the pairs [ai, bi] are distinct.


## 문제 해석

- [207 Course Schedule](https://pangyokyk.github.io/2025/03/29/207-Course-Schedule/) 와 출력만 다른 동일한 문제이다.
- **위상 정렬 문제** 로 선수 과목들이 문제없이 전부 수강할 수 있을때 이것을 위상 정렬하여 vector로 출력하는 문제이다.



## 구현

```cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> ans;
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
            ans.push_back(curr);
            q.pop();

            for(auto next : adj[curr])
            {
                if(--degree[next] == 0)
                {
                    q.push(next);
                }
            }
        }
        if(ans.size() != numCourses)
        {
            return {};
        }
        return ans;
    }
};
```


## 코드 해설

- example 2 를 따라가며 코드를 정리해보자.


```cpp
     0 → 1 → 3
       ↘ ↓
         2
```

- 해당 예시는 이렇게 생긴 구조이다.


```cpp
vector<int> ans; // 위상 정렬한 과목들 저장할 벡터
vector<int> degree(numCourses, 0); // 진입 차수 저장
vector<vector<int>> adj(numCourses, vector<int>()); // 선수 과목과 관련된 다음 과목 저장
```


```cpp
for(auto& p : prerequisites)
{
    adj[p[1]].push_back(p[0]);
    degree[p[0]]++;
}
```

- 노드(선수 과목)에 연결되어있는 노드(선수 과목 다음 들을 수 있는 과목)을 adj 벡터에 저장하고
- 각 노드로 들어오는 간선의 개수인 진입 차수를 degree에 저장해둔다.

```cpp
adj = [ [1,2], [3], [3], [] ] 
degree = [0, 1, 1, 2]  // (0번 노드: 0개, 1번: 1개, 2번: 1개, 3번: 2개)
```


```cpp
for(int i = 0; i < numCourses; i++)
{
    if(degree[i] == 0)
    {
        q.push(i);
    }
}
```

```cpp
q = [0]  // 진입 차수가 0인 노드는 0번 노드뿐!
```

- 진입 차수가 0인 노드를 큐에 집어 넣는다

```cpp
Step 1: 0을 큐에서 제거 → 1과 2의 진입 차수를 감소
q = [1, 2] // degree[1] = 0, degree[2] = 0
```


```cpp
Step 2: 1을 큐에서 제거 → 3의 진입 차수를 감소
q = [2] // degree[3] = 1
```

```cpp
Step 3: 2를 큐에서 제거 → 3의 진입 차수를 감소
q = [3] // degree[3] = 0
```


```cpp
Step 4: 3을 큐에서 제거
q = [] // 모든 노드 방문 완료
```

- 해당 순서로 위상 정렬을 수행하게 된다.
- 결과: [0, 1, 2, 3] (위상 정렬 순서)



```cpp
if(ans.size() != numCourses)
{
    return {};
}
```

- 사이클이 존재하면 위상 정렬을 수행할 수가 없다.
- 이를 확인하는 코드인데 만약 ans의 벡터 크기가 numCourses(수행 가능한 과목) 과 다르다면 사이클이 존재한다는 뜻이다.