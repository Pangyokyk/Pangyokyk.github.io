---
layout: post
title: 199. Binary Tree Right Side View
tags: [Leetcode, Coding Test, dfs, bfs, medium]
date: 2025-02-20 +0800
math: true
toc : true
---




# 199. Binary Tree Right Side View


****


Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

[문제 사이트](https://leetcode.com/problems/binary-tree-right-side-view/description/?envType=study-plan-v2&envId=leetcode-75)

Example 1:

Input: root = [1,2,3,null,5,null,4]

Output: [1,3,4]

Explanation:
![그림](https://assets.leetcode.com/uploads/2024/11/24/tmpd5jn43fs-1.png)


Example 2:

Input: root = [1,2,3,4,null,null,null,5]

Output: [1,3,4,5]

Explanation:
![그림](https://assets.leetcode.com/uploads/2024/11/24/tmpkpe40xeh-1.png)


Example 3:

Input: root = [1,null,3]

Output: [1,3]

Example 4:

Input: root = []

Output: []

 

Constraints:

The number of nodes in the tree is in the range [0, 100].
-100 <= Node.val <= 100



****



## 문제 해석
- 이진 트리가 주어졌을 때 트리를 오른쪽에서 봤을 때 보이는 노드들만 출력하는 문제
- 설명이 모호하지만 노드들이 구체라고 생각하자. 같은 깊이에 있고 오른쪽 노드가 존재한다면 월식처럼 자기 자신이 가려져 보이지않는다고 생각하면 된다.



****



## 구현(dfs)

```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> ans;
        levelSide(root, ans, 0);

        return ans;
    }

    void levelSide(TreeNode* node, vector<int>& ans, int lv) {
        if(!node)   return;
        if(lv == ans.size())
        {
            ans.push_back(node->val);
        }
        levelSide(node->right, ans, lv+1);
        levelSide(node->left, ans, lv+1);
    }
};
```


****



## 코드 해석

```cpp
if(lv == ans.size())
{
    ans.push_back(node->val);
}
```

- **이번 문제에서 가장 핵심 아이디어**
- 가장 오른쪽의 노드를 넣을 때마다 vector<int> ans의 size도 1씩 증가하고 이제 다음 깊이(레벨)의 가장 오른쪽 노드를 찾아야하니 한단계 밑으로 내려가는 것을 이용한 코드이다.


```cpp
levelSide(node->right, ans, lv+1);
levelSide(node->left, ans, lv+1);
```

- 오른쪽 서브트리부터 재귀적으로 호출해야 그 노드를 넣고 size()와 lv 를 증가시켜 넘어가야하기 때문에 **node->right** 부터 재귀적으로 호출한다.



****



## 구현(bfs)

```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        if (!root) {
            return {};
        }
        vector<int> view; //벡터 생성
        queue<TreeNode*> todo; // 큐 생성
        todo.push(root); // 큐에 루트노트 넣어놓기
        while (!todo.empty()) {
            int n = todo.size();
            for (int i = 0; i < n; i++) {
                TreeNode* node = todo.front(); //생성한 객체 node는 큐 todo의 맨 앞의 값을 가짐
                todo.pop(); // 큐 todo 맨 앞의 값 제거
                if (i == n - 1) { // 맨 오른쪽 노드라는 뜻
                    view.push_back(node -> val);
                }
                if (node -> left) {
                    todo.push(node -> left);
                }
                if (node -> right) {
                    todo.push(node -> right);
                }
            }
        }
        return view;
    }
};
```
