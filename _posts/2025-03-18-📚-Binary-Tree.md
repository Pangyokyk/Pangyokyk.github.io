---
layout: post
title: 📚 Binary Tree
tags: [STL, C++, binary tree]
date: 2025-03-18 +0800
math: true
toc : true
---



# 📚 Binary Tree(이진 트리)

- **각 노드가 최대 두 개의 자식 노드를 가질 수 있는 트리 구조**



## 🔎 이진 트리의 기본 개념

- **노드(Node)**
  - 값(val) : 노드에 저장된 데이터
  - 왼쪽 자식(left) : 왼쪽 서브트리를 가리키는 포인터
  - 오른쪽 자식(right) : 오른쪽 서브트리를 가리키는 포인터
- **주요 용어**
  - 루트(root) : 트리의 시작점(최상위 노드)
  - 부모(parent) : 특정 노드를 가리키는 상위 노드
  - 자식(child) : 부모 노드에서 연결된 하위 노드
  - 리프(leaf) : 왼쪽, 오른쪽 자식이 없는 노드
  - 깊이(depth) : 루트에서 특정 노드까지의 거리
  - 높이(height) : 특정 노드에서 가장 깊은 리프까지의 거리




## 💻 C++에서 이진 트리 구현

- **보통 구조체(struct) 또는 클래스(class)로 구현한다.**

### 📌 이진 트리의 노드 구조

```cpp
#include <iostream>
using namespace std;

// 이진 트리 노드 정의
struct TreeNode {
    int val;            // 노드의 값
    TreeNode* left;     // 왼쪽 자식
    TreeNode* right;    // 오른쪽 자식

    // 기본 생성자
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```



### 📌 이진 트리 생성해보기

```cpp
#include <iostream>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int main() {
    // 노드 생성
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);

    return 0;
}
```
- TreeNode의 구조체를 생성한 다음 루트 노드와 그 자식들을 연결시켜주면...

```cpp
        1
       / \
      2   3
     / \
    4   5
```

- **최종적으로 위의 코드로 만든 이진 트리는 이러한 형태를 가지고 있다.**




## 📚 이진 트리 순회(Tree Traversal)

- **이진 트리를 순회하는 방법에는 4가지가 존재한다.**

1. **전위 순회(preorder) :** root -> left -> right
2. **중위 순회(inorder) :** left -> root -> right
3. **후위 순회(postorder) :** left -> right ->root;
4. **레벨 순회(level order) :** BFS 방식(queue 사용)




## 💻 4가지 순회 코드 방법

### 🚀 전위 순회
- **root -> left -> right 방문 순서**

```cpp
void preorder(TreeNode* root) {
    if(!root)   return;
    cout << root->val << " "; // 현재 노드 방문
    preorder(root->left);   // 왼쪽 서브 트리 방문
    preorder(root->right)   // 오른쪽 서브 트리 방문
}
```


### 🚀 중위 순회
- **left -> root -> right 방문 순서**

```cpp
void inorder(TreeNode* root) {
    if(!root)   return;
    inorder(root->left);    //왼쪽 서브 트리 방문
    cout<< root->val << " ";    // 현재 노드 방문
    inorder(root->right);   // 오른쪽 서브 트리 방문
}
```



### 🚀 후위 순회
- **left -> right -> root 방문 순서**

```cpp
void postorder(TreeNode* root)  {
    if(!root)   return;
    postorder(root->left);  // 왼쪽 서브 트리 방문
    postorder(root->right); // 오른쪽 서브 트리 방문
    cout<< root->val << " ";    // 현재 노드 방문
}
```



### 🚀 레벨 순회(BFS)

- **queue를 이용하여 각 레벨 노드를 전부 방문하며 내려간다.**

```cpp
void BFS(TreeNode* root) {
    if(!root)   return;

    queue(TreeNode*) q;
    q.push(root);

    while(!q.empty())
    {
        TreeNode* node = q.front();
        q.pop();
        
        cout << node->val << " ";

        if (node->left) q.push(node->left);
        if (node->right) q.push(node->right);
    }
}
```



## 📚 이진 탐색 트리(Binary search Tree, BST)
- **왼쪽 자식은 부모보다 작고, 오른쪽 자식은 부모보다 큰 값을 가지는 이진 트리**


### 📌 이진 탐색 트리 삽입

```cpp
TreeNode* insertBST(TreeNode* root, int val) {
    if (!root) return new TreeNode(val);
    
    if (val < root->val)
        root->left = insertBST(root->left, val);
    else
        root->right = insertBST(root->right, val);
    
    return root;
}
```


### 📌 이진 탐색 트리 검색
- **값이 더 작아야한다면 왼쪽으로 내려가면 되고 값이 더 커야한다면 오른쪽으로 내려가서 찾으면 된다.**

```cpp
bool searchBST(TreeNode* root, int val) {
    if (!root) return false;
    
    if (root->val == val) return true;
    else if (val < root->val) return searchBST(root->left, val);
    else return searchBST(root->right, val);
}
```




## 🔎 완전 이진 트리(Complete Binary Tree)

- **모든 레벨에서 왼쪽부터 차례대로 노드가 채워진 이진 트리이다.**


### 📌 특징

1. **왼쪽부터 차례대로 노드가 채워져 있어야 한다.**
2. **마지막 레벨을 제외한 모든 레벨에서는 노드가 꽉 차 있어야한다.**
3. 왼쪽 자식이 존재하지 않는데 오른쪽 자식이 존재하는 경우는 없다.



### 📊 형태 예시 

- **완전 이진 트리(O)**
```cpp
       1
      / \
     2   3
    / \  /
   4   5 6
```


- **완전 이진 트리(X)**

```cpp
       1
      / \
     2   3
    /     \
   4       5  ❌ (왼쪽에 5가 먼저 와야 함)
```


### 🔑 완전 이진 트리 알고리즘 문제

[완전 이진 트리 노드 개수 최적화 문제](https://leetcode.com/problems/count-complete-tree-nodes/description/?envType=study-plan-v2&envId=top-interview-150)

- **보통 이진 트리에서 일반적인 재귀를 사용해 숫자를 세면 시간 복잡도는 O(N) 이다.**
- **하지만 <mark>완전 이진 트리</mark> 에서는 높이 비교 최적화를 통해 O((logN)^2) 으로 최적화가 가능하다.**
- 완전 이진트리이고 높이가 h일 때 **2^(h+1) - 1** 의 공식을 사용할 수 있다.