---
layout: post
title: 701. Insert into a Binary Search Tree
tags: [Leetcode, Coding Test, bst, medium]
date: 2025-02-21 +0800
math: true
toc : true
---



# 701. Insert into a Binary Search Tree


****


## 문제 

You are given the root node of a binary search tree (BST) and a value to insert into the tree. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.

Notice that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.

[문제 사이트](https://leetcode.com/problems/insert-into-a-binary-search-tree/description/) 

Example 1:
![그림](https://assets.leetcode.com/uploads/2020/10/05/insertbst.jpg)

Input: root = [4,2,7,1,3], val = 5
Output: [4,2,7,1,3,5]
Explanation: Another accepted tree is:
![그림](https://assets.leetcode.com/uploads/2020/10/05/bst.jpg)


Example 2:

Input: root = [40,20,60,10,30,50,70], val = 25
Output: [40,20,60,10,30,50,70,null,null,25]

Example 3:

Input: root = [4,2,7,1,3,null,null,null,null,null,null], val = 5
Output: [4,2,7,1,3,5]
 

Constraints:

- The number of nodes in the tree will be in the range [0, 10^4].
- -10^8 <= Node.val <= 10^8
- All the values Node.val are unique.
- -10^8 <= val <= 10^8
- It's guaranteed that val does not exist in the original BST.



****


## 문제 해석
- 이진 탐색 트리에서 새로운 val 값의 노드를 추가하는 문제이다.


****


## 구현

```cpp
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if(!root)
        {
            return new TreeNode(val);
        }
        if(root->val < val)
        {
            root->right = insertIntoBST(root->right, val);
        }
        else
        {
            root->left = insertIntoBST(root->left, val);
        }

        return root;
    }
};
```



****


## 코드 해석

```cpp
if(!root)
{
    return new TreeNode(val);
}
```

- 새로 넣을 val 값의 노드를 생성하는 코드
- 이렇게 반환, return을 해줘야 재귀적으로 작동할 때 연결할 수 있다.


```cpp
if(root->val < val)
{
    root->right = insertIntoBST(root->right, val);
}
else
{
    root->left = insertIntoBST(root->left, val);
}
```

- 바로 return insertIntoBST(root, val); 을 하지않는다
- 값을 넣어 반환을 해줘야 새로운 노드가 트리에 추가된다.
