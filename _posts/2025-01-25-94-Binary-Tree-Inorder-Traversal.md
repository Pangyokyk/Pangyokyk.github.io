---
layout: post
title: 94. Binary Tree Inorder Traversal
tags: [Leetcode, Coding Test, dfs, easy]
date: 2025-01-25 +0800
math: true
toc : true
---



# 94. Binary Tree Inorder Traversal


****


## 문제 
Given the root of a binary tree, return the inorder traversal of its nodes' values.

Example 1:

Input: root = [1,null,2,3]

Output: [1,3,2]

Explanation: [그림 설명](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)



Example 2:

Input: root = [1,2,3,4,5,null,8,null,null,6,7,9]

Output: [4,2,6,5,7,1,3,9,8]

Explanation: [그림 설명](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)



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

- binary tree를 output의 결과가 나오도록 순회하는 코드를 짜는 문제이다.


****


## 생각

- 코드를 읽는 방식을 생각해보면 DFS(깊이 우선 탐색)의 **중위 순회** 방식과 동일하다.
- **Follow up** 에는 재귀적으로 코드를 짜보고 반복적으로도 코드를 짜 보라고 한다.


## 구현(self)

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        inorder(root, res);
        return res;
    }
private :
    void inorder(TreeNode* node, vector<int>& res) {
        if(!node) return;

        inorder(node->left, res);
        res.push_back(node->val);
        inorder(node->right, res);
    }
};
```

- 내가 짠 코드는 재귀를 활용한 **중위 순회** 를 구현한 것이다.


## 코드 설명

- **DFS / BFS 요약 참고**


## 구현(Stack)

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stack; //스택 선언, TreeNode의 포인터

        // 처음 노드가 존재하고 스택이 비어있지 않을때 반복
        while(!root == nullptr && !stack.empty())
        {
            //왼쪽 서브트리로 쭉 내려가며 스택에 저장
            while(root != nullptr)
            {
                stack.push(root);
                root = root->left;
            }

            //root에 스택 맨 위 노드 저장만
            root = stack.top();
            // res에 root의 노드의 값 넣기
            res.push_back(root->val);
            //사용한 스택 없애기
            stack.pop();
            //다음 오른쪽 노드로 이동 없으면 스택에서 꺼내서 반복문 진행
            root = root->right;
        }

        return res;
    }
};
```


****


## 후기

BFS의 **재귀적 구현** 과 **반복을 이용한 구현** 을 둘 다 해봤다. 보통 BFS는 재귀 표현을 사용해서 구현한다고 알고 있는데 반복을 이용하여 구현하는 방법도 익혀둬야겠다.