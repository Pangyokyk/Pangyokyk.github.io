---
layout: post
title: 141. Linked List Cycle
tags: [Leetcode, Coding Test, list, two pointers, easy]
date: 2025-03-11 +0800
math: true
toc : true
---



# 141. Linked List Cycle



****


## 문제

Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

[문제 사이트](https://leetcode.com/problems/linked-list-cycle/description/?envType=study-plan-v2&envId=top-interview-150)

Example 1:
![그림](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).


Example 2:
![그림](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.


Example 3:
![그림](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
 

Constraints:

The number of the nodes in the list is in the range [0, 104].
-105 <= Node.val <= 105
pos is -1 or a valid index in the linked-list.
 

**Follow up: Can you solve it using O(1) (i.e. constant) memory?**



****

## 문제 해석

- list의 무한 순환이 가능한지에 대해 물어보는 문제
- **pos** 는 인자로 주어지지 않는다.
- liked-list 는 head 의 정보 말고는 코드를 보기 전까지 알 수 없다고 한다.



****


## 구현

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;

        while(fast != nullptr && fast->next != nullptr)
        {
            fast = fast->next->next;
            slow = slow->next;

            if(fast == slow)
            {
                return true;
            }
        }
        return false;
    }
};
```


****


## 코드 해설

```cpp
ListNode* fast = head;
ListNode* slow = head;
```

- 똑같은 위치에서 시작하는 두 개의 포인터를 생성한다.


```cpp
fast = fast->next->next;
slow = slow->next;
```

- **Floyd’s cycle-finding algorithm** 이라는 **토끼와 거북이 알고리즘** 방법 중 하나라고 한다.
- 즉 토끼 = fast, 거북이 = slow 로 fast가 slow보다 **두배 더 빠른 속도로 움직이니 만약 list가 순환이 된다면 fast가 slow를 만나 추월하게 된다는 아이디어 이다.**