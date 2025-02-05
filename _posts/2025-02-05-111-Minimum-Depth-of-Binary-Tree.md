---
layout: post
title: 111. Minimum Depth of Binary Tree
tags: [Leetcode, Coding Test, dfs, binary search, easy]
date: 2025-02-05 +0800
math: true
toc : true
---



# 111. Minimum Depth of Binary Tree


****


## 문제

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

[문제링크](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)

Example 1:
Input: root = [3,9,20,null,null,15,7]
Output: 2


Example 2:
Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5


Constraints:
The number of nodes in the tree is in the range [0, 10^5].
-1000 <= Node.val <= 1000


****


## 문제 해석
- 이진트리의 최소 깊이를 물어보는 문제이다.


****


## 생각
- 그동안 앞에서 한 것은 'max'를 이용한 최대 깊이 찾기였다.
- 이번에는 왼쪽, 오른쪽 서브 노드중 최소 깊이를 찾아야하니 'min'을 사용해야할 것 같다.
- 재귀적 호출을 이용하면 될 것이다.


****


## 구현

```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(!root)   return 0;
        if(root->left == NULL && root->right == NULL)   return 1;
        if(root->left == NULL)  return minDepth(root->right) + 1;
        if(root->right == NULL) return minDepth(root->left) + 1;
        return min(minDepth(root->left), minDepth(root->right)) + 1;
    }
};
```


****

## 코드 해석

```cpp
if(!root)   return 0;
```
- 노드가 없다면 0을 반환

```cpp
if(root->left == NULL && root->right == NULL)   return 1;
```
- 단말 노드(leaf node)이면 1을 반환한다.

```cpp
if(root->left == NULL)  return minDepth(root->right) + 1;
if(root->right == NULL) return minDepth(root->left) + 1;
```
- 왼쪽 또는 오른쪽 서브 노드가 비어있다면 다시 재귀적으로 존재하는 서브노드 호출

```cpp
return min(minDepth(root->left), minDepth(root->right)) + 1;
```
- 결과적으로 저장해온 숫자들 중 최소값만 저장하며 깊이를 측정한다.