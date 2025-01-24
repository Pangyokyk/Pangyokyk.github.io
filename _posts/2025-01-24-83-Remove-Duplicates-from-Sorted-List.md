---
layout: post
title: 83. Remove Duplicates from Sorted List
tags: [Leetcode, Coding Test, list, easy]
date: 2025-01-24 +0800
math: true
toc : true
---



# 83. Remove Duplicates from Sorted List


****

## 문제 

Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.

Example 1:
Input: head = [1,1,2]
Output: [1,2]

Example 2:
Input: head = [1,1,2,3,3]
Output: [1,2,3]


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
```
 

Constraints:

The number of nodes in the list is in the range [0, 300].
-100 <= Node.val <= 100
The list is guaranteed to be sorted in ascending order.


****


## 문제 해석
오름차순으로 정리되어있는 list를 중복제거하는 문제이다.


****


## 생각

- 중복된 값을 제거
- 이미 정렬되어 있음
- 리스트 형태
- 다음 next 노드값이 이전 값과 같으면 삭제
- 노드는 값+다음 노드주소로 이루어짐
  
헤드 노드는 자기 값(val)과 다음노드 주소(next)를 가지고 있음 > 다음 노드의 값을 알려면 주소로 접근 > 처음값을 가지고 있고 다음 노드 넘어가서 값을 비교해야함


****


## 구현

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == NULL || head->next == nullptr)
        {
            return head;
        }

        ListNode* prev = head;
        ListNode* temp = head->next;

        while(temp != nullptr)
        {
            if(prev->val == temp->val)
            {
                temp = temp->next;
            }
            else
            {
                prev->next = temp;
                prev = temp;
                temp = temp->next;
            }
        }

        prev->next = temp;
        return head;
    }
}
```


****


## 코드 설명
- 먼저 head가 비어있을 경우를 따로 생각해준다.
- prev 는 입력된 head를 추격하기 위한 초기화
- temp 는 head의 다음 노드의 주소를 가짐

```cpp
if(prev->val == temp->val)
{
    temp = temp->next;
}
```

- 만약 현재와 다음 노드의 값이 같다면 다다음 노드로 이동시켜 다시 비교


```cpp
else{
    prev->next = temp;
    prev = temp;
    temp = temp->next;
}
```

- 만약 다르다면 이제 prev, temp 모두 노드 하나씩 이동시켜 비교한다.


```cpp
prev->next = temp;
```

- 마지막 유효 노드를 nullptr로 연결시키기 위함


****


## 후기

list가 나올 때마다 처음 head 노드에서 다음 노드를 접근하는 방식에 다가가는게 어렵게 느껴진다. 많은 list 알고리즘을 풀어서 더욱 익숙해져야겠다.