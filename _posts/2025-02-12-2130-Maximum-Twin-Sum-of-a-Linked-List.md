---
layout: post
title: 2130. Maximum Twin Sum of a Linked List
tags: [Leetcode, Coding Test, list, medium]
date: 2025-02-12 +0800
math: true
toc : true
---




# 2130. Maximum Twin Sum of a Linked List


****


## 문제

In a linked list of size n, where n is even, the ith node (0-indexed) of the linked list is known as the twin of the (n-1-i)th node, if 0 <= i <= (n / 2) - 1.

For example, if n = 4, then node 0 is the twin of node 3, and node 1 is the twin of node 2. These are the only nodes with twins for n = 4.
The twin sum is defined as the sum of a node and its twin.

Given the head of a linked list with even length, return the maximum twin sum of the linked list.

[문제 사이트](https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list/description/?envType=study-plan-v2&envId=leetcode-75)

Example 1:

![이미지](https://assets.leetcode.com/uploads/2021/12/03/eg1drawio.png)
Input: head = [5,4,2,1]
Output: 6
Explanation:
Nodes 0 and 1 are the twins of nodes 3 and 2, respectively. All have twin sum = 6.
There are no other nodes with twins in the linked list.
Thus, the maximum twin sum of the linked list is 6. 



Example 2:

![이미지](https://assets.leetcode.com/uploads/2021/12/03/eg2drawio.png)
Input: head = [4,2,2,3]
Output: 7
Explanation:
The nodes with twins present in this linked list are:
- Node 0 is the twin of node 3 having a twin sum of 4 + 3 = 7.
- Node 1 is the twin of node 2 having a twin sum of 2 + 2 = 4.
Thus, the maximum twin sum of the linked list is max(7, 4) = 7. 


Example 3:

![이미지](https://assets.leetcode.com/uploads/2021/12/03/eg3drawio.png)
Input: head = [1,100000]
Output: 100001
Explanation:
There is only one node with a twin in the linked list having twin sum of 1 + 100000 = 100001.
 

Constraints:

The number of nodes in the list is an even integer in the range [2, 10^5].
1 <= Node.val <= 10^5


****


## 문제 해석
- 리스트 노드의 가장 왼쪽과 가장 오른쪽을 짝을 이뤄 더하며 안쪽 전부 이런식으로 짝을 지어 그 둘의 합의 최대치를 구하는 문제이다.
- 노드는 전부 짝수로 주어지므로 모두 짝이 있다.


****


## 구현

```cpp
class Solution {
public:
    int pairSum(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head; 
        int result = 0;

        while(fast && fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        }

        ListNode* prev = NULL;
        ListNode* cur = NULL;
        while(slow)
        {
            cur = slow->next;
            slow->next = prev;
            prev = slow;
            slow = cur;
        }

        while(prev)
        {
            result = max(result, head->val + prev->val);
            head = head->next;
            prev = prev->next;
        }

        return result;
    }
};
```


****


## 코드 해석


```cpp
while(fast && fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        }
```

- 투포인터 방식으로 slow 보다 2배 빠르게 움직이는 fast 를 설정한다.
- fast 가 끝에 리스트 끝에 도달한다면 slow 의 위치는 리스트의 중앙에 위치하게 된다.(노드 개수가 무조건 짝수라는 조건이 있다.)

```cpp
ListNode* prev = NULL;
ListNode* cur = NULL;
while(slow)
{
    cur = slow->next;
    slow->next = prev;
    prev = slow;
    slow = cur;
}
```

- 저번 문제에서 풀었던 노드 순서를 반대로 뒤집는 방법이다.
- 그림을 그려서 이해하면 쉽다(본인도 그림 4번은 그린듯;:))

```cpp
while(prev)
        {
            result = max(result, head->val + prev->val);
            head = head->next;
            prev = prev->next;
        }
```

- 그렇게 얻은 절반 이후의 뒤집힌 prev 와 나머지 절반 head를 순서대로 더해가며 max를 통해 최대값을 result에 저장해가면 된다. 