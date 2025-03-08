---
layout: post
title: 700. Search in a Binary Search Tree
tags: [Leetcode, Coding Test, bst, easy]
date: 2025-02-20 +0800
math: true
toc : true
---


# 700. Search in a Binary Search Tree



****


## 문제 

You are given the root of a binary search tree (BST) and an integer val.

Find the node in the BST that the node's value equals val and return the subtree rooted with that node. If such a node does not exist, return null.

[문제 사이트](https://leetcode.com/problems/search-in-a-binary-search-tree/description/?envType=study-plan-v2&envId=leetcode-75)

Example 1:
![그림](https://assets.leetcode.com/uploads/2021/01/12/tree1.jpg)

Input: root = [4,2,7,1,3], val = 2
Output: [2,1,3]


Example 2:
![그림](https://assets.leetcode.com/uploads/2021/01/12/tree2.jpg)

Input: root = [4,2,7,1,3], val = 5
Output: []
 

Constraints:
- The number of nodes in the tree is in the range [1, 5000].
- 1 <= Node.val <= 10^7
- root is a binary search tree.
- 1 <= val <= 10^7



****


## 문제 해석

- 전형적인 이진 탐색 트리 (BST) 관련 문제이다.
- val 값의 노드 포함 서브트리까지 출력하라는 문제이다.



****


## 구현

```cpp
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if(!root)   return NULL;

        if(root->val == val)
        {
            return root;
        }
        else if(root->val > val)
        {
            return searchBST(root->left, val);
        }
        else
        {
            return searchBST(root->right, val);
        }
    }
};
```


****



## 코드 해석

```cpp
if(root->val == val)
{
    return root;
}
```

- 정답을 도출하는 코드
- 찾는 값의 해당 노드를 찾으면 return 하면 된다.

```cpp
else if(root->val > val)
{
    return searchBST(root->left, val);
}
```

- **BST는 무조건 상위 노드보다 작은 값은 왼쪽, 큰 값은 오른쪽으로 배치된다.**
- root 노드의 val 값이 찾고자 하는 val 보다 크다면 **val은 왼쪽에 위치하고 있다는 뜻** 이므로 왼쪽 서브 노드로 이동시킨다.


```cpp
else
{
    return searchBST(root->right, val);
}
```

- 나머지 조건은 root->val < val 이므로 **찾고자 하는 val 은 오른쪽 서브 트리에 존재할 것** 이므로 오른쪽 서브트리로 이동시켜 재귀적으로 호출한다.