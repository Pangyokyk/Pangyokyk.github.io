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

**라고 생각했었지만(너무 자기 위주로 생각했다.)**
- list1과 list2 의 첫번째 노드를 비교해본다.
- 크기가 작은 list1의 숫자를 배치해주고 