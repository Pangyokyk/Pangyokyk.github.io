---
layout: post
title: 1161. Maximum Level Sum of a Binary Tree
tags: [Leetcode, Coding Test, dfs, bfs, medium]
date: 2025-02-20 +0800
math: true
toc : true
---




# 1161. Maximum Level Sum of a Binary Tree




****



## 문제 해석

Given the root of a binary tree, the level of its root is 1, the level of its children is 2, and so on.

Return the smallest level x such that the sum of all the values of nodes at level x is maximal.

[문제 사이트](https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree/description/?envType=study-plan-v2&envId=leetcode-75) 

Example 1:

![그림](https://assets.leetcode.com/uploads/2019/05/03/capture.JPG)
Input: root = [1,7,0,7,-8,null,null]
Output: 2
Explanation: 
Level 1 sum = 1.
Level 2 sum = 7 + 0 = 7.
Level 3 sum = 7 + -8 = -1.
So we return the level with the maximum sum which is level 2.


Example 2:

Input: root = [989,null,10250,98693,-89388,null,null,null,-32127]
Output: 2
 

Constraints:
- The number of nodes in the tree is in the range [1, 10^4].

- -10^5 <= Node.val <= 105



****


## 문제 해석
- 같은 깊이에 있는 노드들의 합을 구해 그 합이 최대인 깊이를 찾아내는 문제이다.



****


## 구현(직접)

```cpp
class Solution {
public:
    int maxLevelSum(TreeNode* root) {
        vector<vector<int>> ans;
        levelvec(root, ans, lv);

        vector<long long> sumvec;

        for(auto& row : ans)
        {
            long long sum = 0;
            for(auto a : row)
            {
                sum += a;
            }
            sumvec.push_back(sum);
        }

        int result = 0;
        int maxsum = INT_MIN;
        for(int i = 0; i < sumvec.size(); i++)
        {
            if(maxsum < sumvec[i])
            {
                result = i;
                maxsum = sumvec[i];
            }
        }
        return result + 1;
    }

    void levelvec(TreeNode* node, vector<vector<int>>& ans, int lv) {
        if(!node)   return;
        if(lv == ans.size())
        {
            ans.push_back(vector<int>());
        }
        ans[lv].push_back(node->val);
        levelvec(node->left, ans, lv+1);
        levelvec(node->right, ans, lv+1);
    }
};
```


****

## 코드 해석

```cpp
void levelvec(TreeNode* node, vector<vector<int>>& ans, int lv) {
if(!node)   return;
if(lv == ans.size())
{
    ans.push_back(vector<int>());
}
ans[lv].push_back(node->val);
levelvec(node->left, ans, lv+1);
levelvec(node->right, ans, lv+1);
}
```

- 각 레벨(depth)의 노드들의 합이 들어있는 ans를 만들어주는 함수이다.


```cpp
for(auto& row : ans)
{
    long long sum = 0;
    for(auto a : row)
    {
        sum += a;
    }
    sumvec.push_back(sum);
}
```
- 이중 for문을 사용해 같은 레벨의 노드들의 합을 가지고 있는 벡터 sumvec을 생성해준다.


```cpp
for(int i = 0; i < sumvec.size(); i++)
{
    if(maxsum < sumvec[i])
    {
        result = i;
        maxsum = sumvec[i];
    }
}
```

- for문을 통해 최대 합을 가지고 있는 i번째 인덱스를 찾는 방식이다.


****


### 문제점
- 아직 알고리즘 초보인 나는 문제를 읽고 풀기에 급급하지 '어떻게 하면 더 효율적이고 가독성있는 코드를 짤까' 까지 동시에 생각하며 짜는 것은 힘들다.
- 위에 코드 또한 정답은 맞으나 런타임과 메모리쪽에서 매우 효율성이 떨어진다.
- 먼저 ans라는 같은 레벨의 노드의 벡터를 모으고 다시 합의 모음 sumvec을 생성하니 당연히 효율적이지 못한 것은 당연하다.



****



## 구현(dfs)

```cpp
class Solution {
public:
    int maxLevelSum(TreeNode* root) {
        vector<int> sumvec;
        levelSum(root, sumvec, 0);

        int ans = 0;
        int maxsum = INT_MIN;
        for(int i = 0; i < sumvec.size(); i++)
        {
            if(maxsum < sumvec[i])
            {
                ans = i;
                maxsum = sumvec[i];
            }
        }
        return ans+1;
    }
    void levelSum(TreeNode* node, vector<int>& sumvec, int lv)  {
        if(!node)   return;
        if(sumvec.size() == lv)
        {
            sumvec.push_back(node->val);
        }
        else
        {
            sumvec[lv] += node->val;
        }
        levelSum(node->left, sumvec, lv+1);
        levelSum(node->right, sumvec, lv+1);
    }
};
```


****



## 코드 해석

```cpp
if(sumvec.size() == lv)
{
    sumvec.push_back(node->val);
}
else
{
    sumvec[lv] += node->val;
}
```

- 나는 먼저 같은 레벨에 있는 노드들의 벡터들을 만든 다음에 같은 레벨의 노드들의 합의 벡터를 만들었다
- 위 코드는 바로 합의 벡터를 바로 만드는 방식이다.
- lv(depth)가 같다면 다음 깊이의 첫 노드가 들어온다는 뜻이니 push를 해준다.
- 만약 다르다면 이미 같은 레벨의 다른 노드가 들어가 있는 것이니 이번에 들어오는 노드를 바로 그 lv 인덱스로 가서 더해준다.



****


## 구현(bfs)

```cpp
class Solution {
public:
    int maxLevelSum(TreeNode* root) {
        int maxsum = INT_MIN;
        int ans = 0;
        int level = 0;
        queue<TreeNode*> q;
        q.push(root);

        while(!q.empty())
        {
            level++;
            int cursum = 0;
            for(int i = q.size(); i > 0; --i)
            {
                TreeNode* node = q.front();
                q.pop();
                cursum += node->val;

                if(node->left != nullptr)
                {
                    q.push(node->left);
                }
                if(node->right != nullptr)
                {
                    q.push(node->right);
                }
            }

            if(maxsum < cursum)
            {
                maxsum = cursum;
                ans = level;
            }
        }

        return ans;
    }
};
```

### bfs 문제 tip
- **bfs로 접근해서 이진트리를 풀 때는 그림을 그려서 큐가 어떻게 작동하는지 이해하면 더욱 쉽다.**