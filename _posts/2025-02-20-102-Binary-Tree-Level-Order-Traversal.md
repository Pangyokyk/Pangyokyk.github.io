---
layout: post
title: 102. Binary Tree Level Order Traversal
tags: [Leetcode, Coding Test, dfs, bfs, medium]
date: 2025-02-20 +0800
math: true
toc : true
---



# 102. Binary Tree Level Order Traversal



****



## 문제 

Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

[문제 사이트](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

Example 1:
![그림](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]


Example 2:
Input: root = [1]
Output: [[1]]


Example 3:
Input: root = []
Output: []
 

Constraints:

The number of nodes in the tree is in the range [0, 2000].
-1000 <= Node.val <= 1000


## 문제 해석

- 같은 깊이(depth)에 있는 노드들을 vector<vector<int>> 형태로 출력하는 문제이다.



****


## 구현(dfs)

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        levelNode(root, ans, 0);
        return ans;
    }
    void levelNode(TreeNode* node, vector<vector<int>>& ans, int lv) {
        if(!node)   return;
        if(lv == ans.size())    
        {
            ans.push_back(vector<int>());
        }
        ans[lv].push_back(node->val);
        levelNode(node->left, ans, lv + 1);
        levelNode(node->right, ans, lv + 1);
    }
};
```


****


## 코드 해석

```cpp
if(lv == ans.size())    
{
    ans.push_back(vector<int>());
}
```

- **이번 문제에서 가장 핵심 아이디어**
- 각 레벨(깊이=depth)마다 vector<int>() 를 생성해주는 코드이다.


```cpp
ans[lv].push_back(node->val);
```

- 그럼 노드들을 재귀적으로 호출하며 lv에 맞는 인덱스에 vector로 넣어주는 것이다.
