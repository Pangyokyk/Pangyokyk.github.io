---
layout: post
title: 2. Add Two Numbers
tags: [Leetcode, Coding Test, list, medium]
date: 2025-03-11 +0800
math: true
toc : true
---


# 2. Add Two Numbers



****


## 문제

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

 

Example 1:

![그림](https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg)
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.

Example 2:

Input: l1 = [0], l2 = [0]
Output: [0]

Example 3:

Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
 

Constraints:

- The number of nodes in each linked list is in the range [1, 100].
- 0 <= Node.val <= 9
- It is guaranteed that the list represents a number that does not have leading zeros.



****


## 문제 해석

- 두 개의 비어있지 않는 리스트가 주어진다.
- 두 리스트들의 val값들의 합을 계산해서 출력한다.




****


## 구현

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode();
        ListNode* result = dummy;

        int carry = 0;

        while(l1 != nullptr || l2 != nullptr || carry)
        {
            int sum = 0;

            if(l1 != nullptr)
            {
                sum += l1->val;
                l1 = l1->next;
            }
            if(l2 != nullptr)
            {
                sum += l2->val;
                l2 = l2->next;
            }
            sum += carry;
            carry = sum/10;
            ListNode* newnode = new ListNode(sum%10);
            result->next = newnode;
            result = result->next;
        }
        return dummy->next;
    }
};
```


****



## 코드 해설

```cpp
ListNode* dummy = new ListNode();
ListNode* result = dummy;
```

- 새로운 리스트 객체를 생성하는 코드
- result 는 dummy의 head 노드를 가리키고 있다.
- **왜 바로 result를 새로운 노드로 생성하지 않고 따로 만들어서 그것의 head를 가리키게 할까?**
- **<mark>초기화, 첫 번째 노드를 다룬느 번거로움을 줄이기 위한 기술이기 때문</mark>**



```cpp
while(l1 != nullptr || l2 != nullptr || carry)
```

- 조건에 carry가 들어가는 이유는 만약 마지막 끝 노드에서 carry(올림자)가 남아있다면 그거에 대한 노드도 만들어서 연결해야하기 때문


```cpp
result->next = newnode;
result = result->next;
```

- result 에 새로운 노드를 연결시키고
- result를 다음 노드로 이동 시킨다.