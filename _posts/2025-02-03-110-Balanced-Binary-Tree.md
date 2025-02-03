---
layout: post
title: 110. Balanced Binary Tree
tags: [Leetcode, Coding Test, dfs, binary search, easy]
date: 2025-02-03 +0800
math: true
toc : true
---



# 110. Balanced Binary Tree


****


## 문제
Given a binary tree, determine if it is <mark>height-balanced</mark> (A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.).


Example 1:
Input: root = [3,9,20,null,null,15,7]
Output: true


Example 2:
Input: root = [1,2,2,3,3,null,null,4,4]
Output: false


Example 3:
Input: root = []
Output: true
 

Constraints:

The number of nodes in the tree is in the range [0, 5000].
-10^4 <= Node.val <= 10^4



****


## 문제 해석
이진트리의 왼쪽 서브 노드와 오른쪽 서브 노드의 높이차를 계산하여 '2' 이상이면 'false'를 리턴하는 문제이다.



****


## 생각
- 높이는 어떻게 셀 것인가?
- 높이 = 깊이 우선 탐색(DFS)
- 전에 풀었던 깊이 탐색 문제와 비슷(맨 하위 노드로 내려가서 재귀적으로 숫자 세면서 올라오기)



****


## 구현

```cpp
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        bool flag = true;
        maxDepth(root, flag);
        return flag;
    }

    int maxDepth(TreeNode* root, bool& flag) {
        if(!root)
        {
            return 0;
        }

        int leftDepth = maxDepth(root->left, flag);
        int rightDepth = maxDepth(root->right, flag);

        if(abs(leftDepth - rightDepth) > 1)
        {
            flag = false;
        }
        return max(leftDepth, rightDepth) + 1;
    }
};
```



****

## 코드 해석
- DFS의 높이를 세는 재귀적인 방법이다.

```cpp
int leftDepth = maxDepth(root->left, flag);
int rightDepth = maxDepth(root->right, flag);
```

- 위 코드로 왼쪽, 오른쪽 서브 노드의 맨 아래 노드까지 간다음 


```cpp
if(!root)
{
    return 0;
}
```

- 위 코드로 맨 아래 노드에 0의 값을 가지게 된다.
- 그 후는 **max** 를 통해 위로 하나씩 올라가며 높이를 센다

```cpp
return max(leftDepth, rightDepth) + 1;
```


****


## 후기
풀어봤던 알고리즘에서 조금만 더해진 문제이다. 하지만 자꾸 핵심 알고리즘 방법들을 까먹어서 아이디어에서 막히는 경우가 많은 것 같다. 