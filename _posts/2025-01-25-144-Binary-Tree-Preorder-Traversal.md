---
layout: post
title: 144. Binary Tree Preorder Traversal
tags: [Leetcode, Coding Test, dfs, easy]
date: 2025-01-25 +0800
math: true
toc : true
---



# 144. Binary Tree Preorder Traversal


****


## 문제

Given the root of a binary tree, return the preorder traversal of its nodes' values.

 

Example 1:

Input: root = [1,null,2,3]

Output: [1,2,3]

Explanation: [그림 설명](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)



Example 2:

Input: root = [1,2,3,4,5,null,8,null,null,6,7,9]

Output: [1,2,4,5,6,7,3,8,9]

Explanation: [그림 설명](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)



Example 3:

Input: root = []

Output: []

Example 4:

Input: root = [1]

Output: [1]

 

Constraints:

The number of nodes in the tree is in the range [0, 100].
-100 <= Node.val <= 100
 

**Follow up: Recursive solution is trivial, could you do it iteratively?**


****


## 문제 해석

- DFS 중 **전위 순회** 를 구현하라는 문제이다.


****


## 구현(재귀적)

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        preorder(root, res);
        return res;
    }
private:
    void preorder(TreeNode* node, vector<int>& res) {
        if(!node) return;

        res.push_back(node->val);
        preorder(node->left, res);
        preorder(node->right,res);
    }
};
```


****


## 코드 설명

- [DFS / BFS 요약](https://pangyokyk.github.io/2025/01/25/DFS-BFS/)


****


## 구현(반복문)

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stack;

        if(!root) return res;

        // stack은 root의 주소를 스택으로 하나 넣어두고 있다.
        stack.push(root);

        // 위에서 스택에 하나 넣어둠
        while(!stack.empty())
        {
            TreeNode* node = stack.top();
            stack.pop();
            res.push_back(node->val);

            if(node->right)
            {
                stack.push(node->right);
            }
            if(node->left)
            {
                stack.push(node->left);
            }
        }

        return res;
        
    }
};
```

- 반복문을 사용한 **전위 순회** 를 구현한 것이다.


****


## 후기

**전위 순회** 를 재귀적으로 표현하는 것은 이해해서 암기해뒀는데 반복문을 사용해서 **전위 순회** 를 나타내는 것은 컴퓨팅적 사고, 스택이 어떻게 작동하는지와 순서를 어떻게 짜야 **전위 순회** 가 되는지를 다 알아야한다. 많은 문제를 풀어가며 이러한 경험을 늘려봐야한다.
