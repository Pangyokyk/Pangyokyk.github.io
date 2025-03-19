---
layout: post
title: 101. Symmetric Tree
tags: [Leetcode, Coding Test, binary tree, dfs, bfs, easy]
date: 2025-03-19 +0800
math: true
toc : true
---



# 101. Symmetric Tree


****


## 문제

Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

[문제 사이트](https://leetcode.com/problems/symmetric-tree/description/?envType=study-plan-v2&envId=top-interview-150)

Example 1:

![그림](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)
Input: root = [1,2,2,3,4,4,3]
Output: true


Example 2:
![그림](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

Input: root = [1,2,2,null,3,null,3]
Output: false
 

Constraints:

- The number of nodes in the tree is in the range [1, 1000].
- -100 <= Node.val <= 100
 

**Follow up: Could you solve it both recursively and iteratively?**




****


## 문제 해석

- 루트 노드의 왼쪽 서브트리와 오른쪽 서브트리가 서로 거울상으로 대칭이 되는지 판별하는 문제이다.
- **Follow up :** 문제를 **재귀** 와 **반복** 두 가지 모두 구현할 줄 알아야한다.




****


## 구현(반복)

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
    bool isSymmetric(TreeNode* root) {
        if(!root)   return true;

        queue<TreeNode*> q;
        q.push(root->left);
        q.push(root->right);

        while(!q.empty())
        {
            TreeNode* Lnode = q.front();
            q.pop();
            TreeNode* Rnode = q.front();
            q.pop();

            if(!Lnode && Rnode) return false;
            if(Lnode && !Rnode) return false;
            if(Lnode && Rnode)
            {
                if(Lnode->val == Rnode->val)
                {
                    q.push(Lnode->left);
                    q.push(Rnode->right);
                    q.push(Lnode->right);
                    q.push(Rnode->left);
                }
                else
                {
                    return false;
                }
            }
        }
        return true;
    }
};
```

<mark>**반복으로 문제를 푼다고 결정을 했다면 BFS, DFS 둘 중 뭘 사용해야 더 편하고 깔끔한지 생각해봐야한다.**</mark>

****

## 코드 해설

- **BFS 방식(큐 사용)으로 반복적으로 푼 코드이다.**


```cpp
queue<TreeNode*> q;
q.push(root->left);
q.push(root->right);
```

- 루트노드의 왼쪽 서브트리와 오른쪽 서브트리의 거울상 대칭을 확인해야하니 미리 큐에 루트의 왼쪽 노드와 오른쪽 노드를 넣어둔다.



```cpp
TreeNode* Lnode = q.front();
q.pop();
TreeNode* Rnode = q.front();
q.pop();
```

- 이제 큐에 들어온 순서대로 두 개의 노드를 앞에서 골라 대칭이 맞는지 확인할 것이다.


```cpp
 if(!Lnode && Rnode) return false;
if(Lnode && !Rnode) return false;
if(Lnode && Rnode)
{
    if(Lnode->val == Rnode->val)
    {
        q.push(Lnode->left);
        q.push(Rnode->right);
        q.push(Lnode->right);
        q.push(Rnode->left);
    }
    else
    {
        return false;
    }
}
```

- 왼쪽 노드 또는 오른쪽 노드가 존재하지 않으면 대칭이 아니므로 false를 리턴한다.
- **중요한 부분은 양쪽 노드가 존재하고 그 val값이 같을 때 이다.**
- 위 조건을 만족한다면 총 4개의 노드가 존재하는 것이다.(왼쪽 노드의 자식 두개 + 오른쪽 노드 자식 두개)
- 그것을 넣는 **순서** 가 중요한데 **거울상** 으로 대칭이 되야하므로 위 코드의 순서를 꼭 지켜서 넣어줘야한다.





****


## 구현(재귀적)


```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(!root)   return true;

        return Symmetric(root->left, root->right);
    }

    bool Symmetric(TreeNode* Lnode, TreeNode* Rnode) {
        if(!Lnode && !Rnode)    return true;

        if(Lnode && Rnode && Lnode->val == Rnode->val)
        {
            return Symmetric(Lnode->left, Rnode->right) && Symmetric(Lnode->right, Rnode->left);
        }
        return false;
    }
};
```


<mark>**재귀로 풀어야한다면 기저조건 작성 -> 자기 자신의 함수를 재호출 할때 어떤 방식으로 작동하게 할 것인지 생각 -> 필요한 변수들 생각**</mark>



****

## 코드 해설


```cpp
bool Symmetric(TreeNode* Lnode, TreeNode* Rnode) {
}
```

- 따로 함수를 만들어 재귀적으로 문제를 푼다.

```cpp
if(!Lnode && !Rnode)    return true;
```
- 재귀의 기저조건이다.
- **비교하는 두 노드가 NULL 이라는 것은 재귀호출이 이진 트리의 끝까지 성공적으로 호출이 되었다는 것으로 true를 리턴한다.**


```cpp
if(Lnode && Rnode && Lnode->val == Rnode->val)
{
    return Symmetric(Lnode->left, Rnode->right) && Symmetric(Lnode->right, Rnode->left);
}
```

- 문제에서는 왼쪽, 오른쪽 서브트리를 거울상으로 비교하고 그의 값이 맞는지를 따져야 하는 문제이기 때문에 이를 재귀적으로 호출한다는 생각을 한다.
- **거울상 대칭을 위해 총 두번의 비교를 해야하기 때문에 재귀도 두번이 필요하다.**