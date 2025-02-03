---
layout: post
title: 108. Convert Sorted Array to Binary Search Tree
tags: [Leetcode, Coding Test, array, easy]
date: 2025-02-03 +0800
math: true
toc : true
---



# 108. Convert Sorted Array to Binary Search Tree


****


## 문제 

Given an integer array nums where the elements are sorted in ascending order, convert it to a height-balanced binary search tree.

 

Example 1:

Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:


Example 2:

Input: nums = [1,3]
Output: [3,1]
Explanation: [1,null,3] and [3,1] are both height-balanced BSTs.
 

Constraints:

1 <= nums.length <= 10^4
-10^4 <= nums[i] <= 10^4
nums is sorted in a strictly increasing order.



****


## 문제 해석

균형있는 이진탐색 트리에 관한 문제로 Input으로 숫자들이 들어오면 이것을 Output의 결과에 맞도록 출력하는 문제이다.


****


## 생각

- 사실 문제에 'binary search' 를 보고 어떤 아이디어로 풀어야할지 감이 잡히지 않았다.
- **Example1** Output은 Input을 오름차순으로 정리하여 출력되는데 ** Example2** 를 보면 오름차순이 아닌 경우도 된다고 하니 문제의 감을 잡는데 어려웠다.



****


## 구현

```cpp
class Solution {
public :
    TreeNode* sortedArrayToBST(vector<int> &nums) {
        return binary_Search(nums, 0, nums.size()-1);
    }

    TreeNode* binary_Search(vetcot<int> &nums, int low, int high) {
        if(low > high)
            return NULL;

        int mid = low + (high - low)/2;
        TreeNode* root = new TreeNode();
        root->val = nums[mid];
        root->left = binary_Search(nums, low, mid - 1);
        root->right = binary_Search(nums, mid+1, high);
    }
};
```


****



## 코드 해석
- 이 문제는 BST(넓이 우선 탐색)에 관한 문제였다.
- Input으로 들어온 숫자들의 **가운데(mid)** 를 처음 root로 지정하고 가운데 숫자를 기준으로 왼쪽, 오른쪽 서브 트리로 나누어 계속 가운데를 지정해 트리를 만드는 문제였다.
- [문제 사이트에 친절한 움직이는 그림 설명이 있다](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)



****

## 후기
**설날 + 독감으로 일주일을 쉬어버렸는데 초보자인만큼 그동안 감이 많이 떨어진 것 같다. 못한만큼 더 열심히 복습하고 공부해야겠다.**