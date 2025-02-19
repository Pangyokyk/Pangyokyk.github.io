---
layout: post
title: 1372. Longest ZigZag Path in a Binary Tree
tags: [Leetcode, Coding Test, dfs, dynamic programming, medium]
date: 2025-02-19 +0800
math: true
toc : true
---




# 1372. Longest ZigZag Path in a Binary Tree



****


## 문제 

You are given the root of a binary tree.

A ZigZag path for a binary tree is defined as follow:

- Choose any node in the binary tree and a direction (right or left).


- If the current direction is right, move to the right child of the 

- current node; otherwise, move to the left child.

- Change the direction from right to left or from left to right.

- Repeat the second and third steps until you can't move in the tree.


Zigzag length is defined as the number of nodes visited - 1. (A single node has a length of 0).
Return the longest ZigZag path contained in that tree.

[문제 사이트](https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/description/?envType=study-plan-v2&envId=leetcode-75)

Example 1:

![그림](https://assets.leetcode.com/uploads/2020/01/22/sample_1_1702.png)
Input: root = [1,null,1,1,1,null,null,1,1,null,1,null,null,null,1]
Output: 3
Explanation: Longest ZigZag path in blue nodes (right -> left -> right).


Example 2:

![그림](https://assets.leetcode.com/uploads/2020/01/22/sample_2_1702.png)
Input: root = [1,1,1,null,1,null,null,1,1,null,1]
Output: 4
Explanation: Longest ZigZag path in blue nodes (left -> right -> left -> right).


Example 3:
Input: root = [1]
Output: 0
 

Constraints:

The number of nodes in the tree is in the range [1, 5 * 10^4].
1 <= Node.val <= 100


****


## 문제 해석

- 이진 트리에서 지그재그로 이동할 수 있는 최대 횟수를 물어보는 문제


****


## 구현

```cpp
class Solution {
public:
    int longestZigZag(TreeNode* root) {
        ZigZag(root, true, 0);

        return count;
    }

    int count = 0;
    void ZigZag(TreeNode* node, bool goleft, int step) {
        if(!node)   return;
        count = max(count, step);
        if(goleft) // 이전에 오른쪽으로 갔었다는 것이므로 왼쪽으로 가야함
        {
            ZigZag(node->left, false, step + 1);
            ZigZag(node->right, true, 1);
        }
        else
        {
            ZigZag(node->left, false, 1);
            ZigZag(node->right, true, step+1);
        }
    }
};
```