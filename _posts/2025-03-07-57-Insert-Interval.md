---
layout: post
title: 57. Insert Interval
tags: [Leetcode, Coding Test, array, medium]
date: 2025-03-07 +0800
math: true
toc : true
---



# 57. Insert Interval


****


## 문제 

You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.

Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return intervals after the insertion.

Note that you don't need to modify intervals in-place. You can make a new array and return it.

[문제 사이트](https://leetcode.com/problems/insert-interval/description/?envType=study-plan-v2&envId=top-interview-150)

Example 1:

Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]

Example 2:

Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
 

Constraints:

- 0 <= intervals.length <= 10^4
- intervals[i].length == 2
- 0 <= starti <= endi <= 10^5
- intervals is sorted by starti in ascending order.
- newInterval.length == 2
- 0 <= start <= end <= 10^5



****


## 문제 해석
- intervals 에서 서로 겹치는 구간이 존재한다면 합친다.
- newInterval 의 구간을 삽입하여 그 결과를 출력한다.



****


## 구현(self)

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        int n = intervals.size();
        bool insert = false;

        for(int i = n - 1; i >= 0; i--)
        {
            if(intervals[i][0] < newInterval[0])
            {
                intervals.insert(intervals.begin() + i + 1, newInterval);
                insert = true;
                break;
            }
        }
        if(!insert)
        {
            intervals.insert(intervals.begin(), newInterval);
        }

        if(intervals.empty())   return {newInterval};
        vector<vector<int>> ans;
        ans.push_back(intervals[0]);

        for(int i = 1; i < intervals.size(); i++)
        {
            if(ans.back()[1] >= intervals[i][0])
            {
                ans.back()[1] = max(ans.back()[1], intervals[i][1]);
            }
            else
            {
                ans.push_back(intervals[i]);
            }
        }
        return ans;
    }
};
```



****



## 코드 해설

```cpp
for(int i = n - 1; i >= 0; i--)
{
    if(intervals[i][0] < newInterval[0])
    {
        intervals.insert(intervals.begin() + i + 1, newInterval);
        insert = true;
        break;
    }
}
```
- newinterval의 삽입 자리를 찾는 과정이다.
- 문제에서 intervals가 이미 오름차순으로 정렬되어있다는 조건이다.


```cpp
if(!insert)
{
    intervals.insert(intervals.begin(), newInterval);
}
```
- 만약 찾지 못했다는 것은 newInterval[0]이 가장 작다는 것이므로 맨 앞에 삽입해주면 된다.


```cpp
for(int i = 1; i < intervals.size(); i++)
{
    if(ans.back()[1] >= intervals[i][0])
    {
        ans.back()[1] = max(ans.back()[1], intervals[i][1]);
    }
    else
    {
        ans.push_back(intervals[i]);
    }
}
```
- 겹치는 구간을 합쳐주는 코드




## 구현(정답)

**물론 내가 작성한 것도 정답이지만 더 깔끔한 코드를 공부하고 싶었다.**

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        int n = intervals.size();
        int i = 0;
        vector<vector<int>> res;

        //newInterval 합치기 전 newInterval과 겹치는 구간이 존재하지 않을 때
        while(i < n && intervals[i][1] < newInterval[0])
        {
            res.push_back(intervals[i]);
            i++;
        }

        //newInterval과 겹치는 구간이 존재할 때
        while(i < n && newInterval[1] >= intervals[i][0])
        {
            newInterval[0] = min(newInterval[0], intervals[i][0]); //앞부분은 최소로
            newInterval[1] = max(newInterval[1], intervals[i][1]); //뒷부분은 최대로 
            i++;
        }
        res.push_back(newInterval);

        // 그 후 나머지 부분 전부 넣어주기
        while(i<n)
        {
            res.push_back(intervals[i]);
            i++;
        }
        return res;
    }
};
```