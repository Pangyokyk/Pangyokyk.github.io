---
layout: post
title: 138. Copy List with Random Pointer
tags: [Leetcode, Coding Test, list, medium]
date: 2025-03-12 +0800
math: true
toc : true
---



# 138. Copy List with Random Pointer



****


## 문제

A linked list of length n is given such that each node contains an additional random pointer, which could point to any node in the list, or null.

Construct a deep copy of the list. The deep copy should consist of exactly n brand new nodes, where each new node has its value set to the value of its corresponding original node. Both the next and random pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list.**

For example, if there are two nodes X and Y in the original list, where X.random --> Y, then for the corresponding two nodes x and y in the copied list, x.random --> y.

Return the head of the copied linked list.

The linked list is represented in the input/output as a list of n nodes. Each node is represented as a pair of [val, random_index] where:

val: an integer representing Node.val
random_index: the index of the node (range from 0 to n-1) that the random pointer points to, or null if it does not point to any node.
Your code will only be given the head of the original linked list.

[문제 사이트](https://leetcode.com/problems/copy-list-with-random-pointer/description/?envType=study-plan-v2&envId=top-interview-150) 

Example 1:
![그림](https://assets.leetcode.com/uploads/2019/12/18/e1.png)

Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]


Example 2:
![그림](https://assets.leetcode.com/uploads/2019/12/18/e2.png)

Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]


Example 3:
![그림](https://assets.leetcode.com/uploads/2019/12/18/e3.png)

Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]
 

Constraints:

- 0 <= n <= 1000
- -10^4 <= Node.val <= 10^4
- Node.random is null or is pointing to some node in the linked list.



****

## 문제 해석
- 연결 리스트는 각 val 값과 자기 자신을 제외한 무작위 노드랑 연결되어있는 random이 존재한다.
- 각 노드의 random은 nullptr과 연결되어있을 수도 있다.
- **None of the pointers in the new list should point to nodes in the original list.** -> **새 목록의 포인터 중 어느 것도 원래 목록의 노드를 가리켜서는 안 됩니다.**



****


## 코드 구현

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(!head)   return nullptr;

        //원본 노드 복사 + 연결맺어주기
        Node* curr = head;
        while(curr)
        {
            Node* copy = new Node(curr->val);
            copy->next = curr->next;
            curr->next = copy;
            curr = copy->next;
        }

        //원본 random을 복사본에 넣어주기
        curr = head;
        while(curr)
        {
            //random이 존재할 때만 넣어야함, null도 존재함
            if(curr->random)
            {
                curr->next->random = curr->random->next;
            }
            curr = curr->next->next;
        }

        //원본 훼손 복구 + 복사본을 출력
        curr = head;
        Node* dummy = new Node(0);
        Node* new_node = dummy;
        while(curr)
        {
            new_node->next = curr->next;
            //복사본을 가리키고있던거 복구
            curr->next = curr->next->next;
            curr = curr->next;
            new_node = new_node->next; 
        }
        return dummy->next;
    }
};
```


****


## 코드 해설
- 문제가 원본을 훼손하지 않으며 똑같이 복사하는 문제이다.

```cpp
head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
```
- 설명을 위해 위 예시를 둔다.

```cpp
Node* curr = head;
while(curr)
{
    Node* copy = new Node(curr->val);
    copy->next = curr->next;
    curr->next = copy;
    curr = copy->next;
}
```
- 원본 노드를 지나갈 때마다 복사후 복사본과 원본을 연결한다.

```cpp
7->7'->13->13'->11->11'->10->10'->1->1'
```


```cpp
curr = head;
while(curr)
{
    if(curr->random)
    {
        curr->next->random = curr->random->next;
    }
    curr = curr->next->next;
}
```
- 원본의 val값과 같이 붙어있는 random 또한 복사해준다.
- 단, random은 null인 값도 존재하니 if문으로 이를 걸러줘야한다.


```cpp
curr = head;
Node* dummy = new Node(0);
Node* new_node = dummy;
while(curr)
{
    new_node->next = curr->next;
    curr->next = curr->next->next;
    curr = curr->next;
    new_node = new_node->next; 
}
```

- 지금 head는 '원본->복사본->원본->본사본...'의 형태로 변형되어있다.
- 문제에서 **원본을 훼손하지말 것** 이라는 조건이 있었다.
- 그렇기 때문에 원본과 복사본을 분리해주는 작업을 한다.
- **나는 while문을 그림을 그려가며 작동을 이해했다. 연결 리스트는 무엇을 기준으로 잡고 어디에 연결할 것인지를 잘 파악해야하는 알고리즘이라는 걸 문제들을 풀어가며 파악했다.**