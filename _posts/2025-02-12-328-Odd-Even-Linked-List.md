---
layout: post
title: 328. Odd Even Linked List
tags: [Leetcode, Coding Test, list, medium]
date: 2025-02-12 +0800
math: true
toc : true
---



# 328. Odd Even Linked List


****


## 문제

Given the head of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return the reordered list.

The first node is considered odd, and the second node is even, and so on.

Note that the relative order inside both the even and odd groups should remain as it was in the input.

You must solve the problem in O(1) extra space complexity and O(n) time complexity.

 [문제 사이트](https://leetcode.com/problems/odd-even-linked-list/description/?envType=study-plan-v2&envId=leetcode-75)

Example 1:

![이미지](https://assets.leetcode.com/uploads/2021/03/10/oddeven-linked-list.jpg)
Input: head = [1,2,3,4,5]
Output: [1,3,5,2,4]


Example 2:

![이미지](https://assets.leetcode.com/uploads/2021/03/10/oddeven2-linked-list.jpg)
Input: head = [2,1,3,5,6,4,7]
Output: [2,3,6,7,1,5,4]
 

Constraints:

The number of nodes in the linked list is in the range [0, 10^4].
-10^6 <= Node.val <= 10^6


****


## 문제 해석
- 홀수, 짝수번째의 노드들을 따로 분리한다
- 홀수 리스트와 짝수 리스트를 서로 이어붙인 것을 반환한다.


****


## 구현

```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head == nullptr || head->next == nullptr || head->next->next == nullptr)
        {
            return head;
        }

        ListNode* odd = head;
        ListNode* even = head->next;
        ListNode* even_start = head->next;

        while(odd->next && even->next)
        {
            odd->next = even->next;
            even->next = odd->next->next;
            odd = odd->next;
            even = even->next;
        }
        odd->next = even_start;
        return head;
    }
};
```


****


## 코드 해석

```cpp
if(head == nullptr || head->next == nullptr || head->next->next == nullptr)
        {
            return head;
        }
```

- head의 노드 개수가 2개까지는 변환도 필요없이 자기자신이 곧 결과물이다.


```cpp
ListNode* odd = head;
ListNode* even = head->next;
ListNode* even_start = head->next;
```

- odd와 even의 시작지점을 정해준다.
- 마지막에 odd 리스트와 even 리스트를 서로 붙여줘야하기 때문에 처음 even의 시작위치를 저장시키는 even_start를 선언한다.


```cpp
while(odd->next && even->next)
        {
            odd->next = even->next;
            even->next = odd->next->next;
            odd = odd->next;
            even = even->next;
        }
```

- **이번 문제에서 가장 핵심적인 원리 부분**
- 그림을 그려 이해해보는 것이 더 좋다.
- 간단히 odd 와 even 의 다음 노드들의 위치를 어떻게 지정하는지에 대한 코드라고 생각하면 된다.