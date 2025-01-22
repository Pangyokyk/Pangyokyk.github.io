---
layout: post
title: 21. Merge Two Sorted Lists
tags: [Leetcode, Coding Test, list, easy]
date: 2025-01-21 +0800
math: true
toc : true
---


# 21. Merge Two Sorted Lists


****


## 문제

You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
Example 2:

Input: list1 = [], list2 = []
Output: []
Example 3:

Input: list1 = [], list2 = [0]
Output: [0]


```cpp
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
```

****

## 문제 해석
- **리스트 합병 + 정렬 문제이다.** list1 과 list2이 Input으로 들어오면 숫자가 더 작은 노드가 앞으로 와 오름차순으로 정리시키면 된다.
- ListNode의 구조가 주어져있다. **첫번째 인자 int x = val, int가 저장된다** **두번째 인자는 다음 노드를 가리키는 next 포인터이다.**


****


## 생각
1. list1의 마지막 노드가 list2의 첫번째 노드를 가리키도록 한다.
2. 그리고 리스트를 정렬한다.

**라고 생각했었지만(컴퓨팅적 사고를 기르자)**
- list1과 list2 의 첫번째 노드를 비교해본다.
- 작거나 같은 리스트의 노드를 리턴하고 재귀적으로 다시 함수를 호출해서 비교를 계속 하면 될 것이다.
- 만약 list1 과 list2 둘중 하나가 비어있으면 비어있지 않은 리스트를 호출하는 예문도 만들어둬야할 것이다.


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
 //문제에서 주어진 ListNode의 구조체

class Solution{
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        //list1, list2 둘중 하나가 비어있는 경우
        if(list1 == NULL)
        {
            return list2;
        }
        if(list2 == NULL)
        {
            return list1;
        }

        //list1과 list2 노드들 비교
        //list1 값이 더 작을 경우
        if(list1 -> val <= list2 -> val)
        {
            list1 -> next = mergeTwoLists(list1->next, list2);
            return list1;
        }
        else //list2 값이 더 작을 경우
        {
            list2 -> next = mergeTwoLists(list1, list2 -> next)
            return list2;
        }
    }
 };
```


****


## 코드관찰
- 재귀적으로 접근하는 방법에 대해 생각해보는 문제였다.
- **List** 관련 문제를 풀 때는 노드의 구조에 대해 생각해 노드에 어떻게 접근해서 풀 것인지를 잘 생각해봐야 할 것 같다.