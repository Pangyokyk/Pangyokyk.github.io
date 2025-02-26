---
layout: post
title: 380. Insert Delete GetRandom O(1)
tags: [Leetcode, Coding Test, array, hash table, medium]
date: 2025-02-26 +0800
math: true
toc : true
---


# 380. Insert Delete GetRandom O(1)



****


## 문제
Implement the RandomizedSet class:

- RandomizedSet() Initializes the RandomizedSet object.
- bool insert(int val) Inserts an item val into the set if not present. Returns true if the item was not present, false otherwise.
- bool remove(int val) Removes an item val from the set if present. Returns true if the item was present, false otherwise.
- int getRandom() Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the same probability of being returned.


You must implement the functions of the class such that each function works in average O(1) time complexity.

[문제 사이트](https://leetcode.com/problems/insert-delete-getrandom-o1/description/?envType=study-plan-v2&envId=top-interview-150)

Example 1:

Input
["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
[[], [1], [2], [2], [], [1], [2], []]
Output
[null, true, false, true, 2, true, false, 2]

Explanation
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomizedSet.remove(2); // Returns false as 2 does not exist in the set.
randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2].
randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly.
randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2].
randomizedSet.insert(2); // 2 was already in the set, so return false.
randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.
 

Constraints:

- -2^31 <= val <= 2^31 - 1
- At most 2 * 10^5 calls will be made to insert, remove, and getRandom.
- There will be at least one element in the data structure when getRandom is called.



****


## 문제 해석

- RandomizedSet() 은 객체 초기화 함수
- bool inset() 은 val값을 중복을 확인하며 넣는 함수
- bool remove() 은 val값이 존재하면 지우는 함수
- getRandom() 은 호출 시점에서 존재하는 원소들 중 동일한 확률로 하나의 수를 뽑는 함수
- 반드시 시간 복잡도가 \(O(1)\) 되도록 설계해야한다.



****


## 구현

```cpp
class RandomizedSet {
public:
    RandomizedSet() {
        
    }
    
    bool insert(int val) {
        if(m.find(val) != m.end())  return false;
        nums.push_back(val);
        m[val] = nums.size()-1;
        return true;
    }
    
    bool remove(int val) {
        if(m.find(val) == m.end())  return false;
        int last = nums.back();
        m[last] = m[val];
        nums[m[val]] = last;
        nums.pop_back();
        m.erase(val);
        return true;
    }
    
    int getRandom() {
        return nums[rand() % nums.size()];
    }
private:
    vector<int> nums;
    unordered_map<int, int> m;
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```



****


## 코드 해석

- **remove() 를 \(O(1\)) 의 시간 복잡도로 코드를 짜는 아이디어가 어려운 문제이다**


### bool insert() 부분

```cpp
if(m.find(val) != m.end())  return false;
```
- hash table 에서 **find() 메소드는 value를 호출하고 못찾으면 end()를 호출한다.**
- 즉 m.find(val) 이 m.end() 가 아니라면 중복이 있다는 뜻이므로 false를 리턴하면 된다.


```cpp
nums.push_back(val);
```
- vector nums 는 getRandom() 함수를 위한 벡터이다. 

```cpp
m[val] = nums.size()-1;
```
- 만약 val = 10 이라면 m[10] = 1 - 1 = 0 이다.
- **10은 key** 이고 **m[10] = 0 은 value** 를 지칭하는 것이다.
- <mark>m = [ 10 : 0 ] 의 형태를 취하고 있다.</mark>



### bool remove() 부분

```cpp
if(m.find(val) == m.end())  return false;
```
- insert와 비슷하게 m.find(val) 가 end() 라면 지울 val값을 찾지 못했다라는 뜻이므로 false를 리턴한다.

```cpp
int last = nums.back();
m[last] = m[val];
nums[m[val]] = last;
nums.pop_back();
m.erase(val);
```

- **이번 문제에서 가장 핵심적인 아이디어**
- 지울 val 의 key를 덮어씌워서 교체(swap방식)한다음 지우는 메커니즘이다.
- 예시로 nums = [10, 20, 30], m = [ 10 : 0, 20 : 1, 30 : 2], val = 20 이라고 해보자.

```cpp
int last = nums.back(); // last = 30
m[last] = m[val]; // m[30] = m[20] = 1, m = [ 10 : 0, 20 : 1, 30 : 1]
nums[m[val]] = last; // nums[1] = 30, nums = [10, 30, 30];
nums.pop_back(); // nums = [10, 30];
m.erase(val); // m.erase(20), m = [ 10 : 0, 30 : 1]
```


### int getRandom() 부분
```cpp
return nums[rand % nums.size()];
```

- nums 벡터에서 범위 nums.size() 만큼 중 랜덤하게 숫자를 뽑는 코드
- **암기**