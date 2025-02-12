---
layout: post
title: 206. Reverse Linked List
tags: [Leetcode, Coding Test, list, easy]
date: 2025-02-12 +0800
math: true
toc : true
---



# 206. Reverse Linked List



****



## 문제 

Given the head of a singly linked list, reverse the list, and return the reversed list.

[문제 사이트](https://leetcode.com/problems/reverse-linked-list/description/?envType=study-plan-v2&envId=leetcode-75)

Example 1:

![그림](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]


Example 2:

![그림](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)
Input: head = [1,2]
Output: [2,1]


Example 3:
Input: head = []
Output: []
 

Constraints:

The number of nodes in the list is the range [0, 5000].
-5000 <= Node.val <= 5000
 

**Follow up: A linked list can be reversed either iteratively or recursively. Could you implement both?**


****


## 문제 해석
- 주어진 리스트를 reverse 하는 문제
- **Follow up** 은 문제를 반복적과 재귀적 두가지 방식으로 풀라고 되어있다.


****



## 구현(반복적)

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
        {
            return head;
        }

        ListNode* prev = NULL;
        ListNode* cur = NULL;

        while(head)
        {
            cur = head->next;
            head->next = prev;
            prev = head;
            head = cur;
        }
        return prev;
    }
};
```


****


## 코드 해석

```cpp
ListNode* prev = NULL;
ListNode* cur = NULL;
```

- **반복적으로 풀 때 핵심 아이디어**
- 최종적으로 역순으로 출력할 prev
- 현재 노드의 위치를 옮기기 위한 cur

```cpp
while(head)
        {
            cur = head->next;
            head->next = prev;
            prev = head;
            head = cur;
        }
```

- **여기도 핵심적인 아이디어 부분이다**
- 짧은 예시로 그림을 그려가보며 생각하면 최종적으로 저러한 구조를 띄는 코드가 완성된다.(본인도 그림을 그린 후 코드를 작성했다.)



****


## 구현(재귀적)

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
        {
            return head;
        }

        ListNode* prev = NULL;
        ListNode* head2 = reverseList(head->next);
        head->next->next = head;
        head->next = prev;

        return head2;
    }
};
```



****


## 코드 해석

```cpp
ListNode* head2 = reverseList(head->next);
```

- 재귀적으로 함수를 호출해서 푸는 방법이다.
- 재귀는 스택처럼 작동한다는 것을 잊지말자.
- 본인은 재귀적 방법에 약해 자세히 예시로 정리해봤다.


```cpp
head = [1,2,3,4]
```
- list가 이런식으로 주어졌다고 하자.

```cpp
head2 = reverseList(2);
head2 = reverseList(3);
head2 = reverseList(4);
```

- 위의 순서대로 재귀적 호출이 될 것이다.
- reverseList(4)가 호출되면 4->next = nullptr 로 처음 if문에 의해 걸러져 head2 = 4 를 저장하고 reverseList(3)으로 넘어간다.

```cpp
head2 = reverseList(3);
```
- head2 = 4 이고 head = 3 head->next = 4 인 상황이다.

```cpp
head->next->next = head;
```

- head->next = 4, head = 3 이므로

```cpp
4->next = 3
```

- 위의 식이 성립하게 된다.
- 즉 4노드 다음에 3노드를 연결 시켜준 것이다.

```cpp
head->next = prev;
```

- head = 3 이므로 3 다음 노드가 NULL을 가리키도록 하고 다음 reverse(2) 로 진입하여 이를 반복한다.

- 뭔가 끼워맞추기 식으로 설명이 된 것 같지만 이를 그림으로 그려보고 어떻게 재귀적으로 불러왔을 때 식을 세워야 작동할지 생각해보면 된다.
- 재귀적 함수 짜는 방법을 더 연습해야겠다.
- 
