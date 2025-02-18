---
layout: post
title: 872. Leaf-Similar Trees
tags: [Leetcode, Coding Test, dfs, easy]
date: 2025-02-18 +0800
math: true
toc : true
---



# 872. Leaf-Similar Trees


****


## 문제 

Consider all the leaves of a binary tree, from left to right order, the values of those leaves form a leaf value sequence.

![그림](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/16/tree.png)

For example, in the given tree above, the leaf value sequence is (6, 7, 4, 9, 8).

Two binary trees are considered leaf-similar if their leaf value sequence is the same.

Return true if and only if the two given trees with head nodes root1 and root2 are leaf-similar.

[문제 사이트](https://leetcode.com/problems/leaf-similar-trees/description/?envType=study-plan-v2&envId=leetcode-75)

Example 1:

![그림](https://assets.leetcode.com/uploads/2020/09/03/leaf-similar-1.jpg)
Input: root1 = [3,5,1,6,2,9,8,null,null,7,4], root2 = [3,5,1,6,7,4,2,null,null,null,null,null,null,9,8]
Output: true


Example 2:

![그림](https://assets.leetcode.com/uploads/2020/09/03/leaf-similar-2.jpg)
Input: root1 = [1,2,3], root2 = [1,3,2]
Output: false
 

Constraints:

- The number of nodes in each tree will be in the range [1, 200].
- Both of the given trees will have values in the range [0, 200].



****



## 문제 해석
- 서로 다른 이진 트리의 leaf노드가 같은지 묻는 문제이다.
- 순서까지 고려해 동일한지 판단해야 한다.


****



## 구현

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool leafSimilar(TreeNode* root1, TreeNode* root2) {
        vector<int> r1;
        vector<int> r2;

        Myvector(root1, r1);
        Myvector(root2, r2);

        return (r1 == r2);
    }

    vector<int> Myvector(TreeNode* root, vector<int>& v) {
        if(!root)   return v;
        if(root->left == nullptr && root->right == nullptr)
        {
            v.push_back(root->val);
        }
        Myvector(root->left, v);
        Myvector(root->right, v);

        return v;
    }
};
```



****


## 코드 해석

```cpp
if(root->left == nullptr && root->right == nullptr)
        {
            v.push_back(root->val);
        }
Myvector(root->left, v);
Myvector(root->right, v);
```

- leaf 노드는 왼쪽, 오른쪽 자식노드가 없는 노드이다.
- 재귀적 호출을 통해 leaf 노드까지 도달하여 확인하고 벡터에 넣어준다.