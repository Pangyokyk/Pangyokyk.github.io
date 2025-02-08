---
layout: post
title: Hash table
tags: [C++, algorithm, hash table]
date: 2025-02-08 +0800
math: true
toc : true
---


# Hash table


****

## 📚 Hash table 이란?
- **키-값(key-value)** 쌍 데이터를 빠르게 저장하고 검색할 수 있는 데이터 구조이다.
- 내부적으로 해시 함수를 사용해 데이터를 특정 **버킷(bucket)** 에 매핑하여 \(O(1\)) 수준의 평군 검색 시간을 보장한다.


****


## 📚 Hash table 구현 컨테이너

1. unordered_map
   - 키-값(key-value) 쌍을 저장하는 해시 테이블

2. unordered_set
   - **<mark>중복</mark>** 되지 않는 값만 저장하는 해시 테이블


### 📚 구현 예제

```cpp
#include <iostream>
#include <unordered_map>

using namespace std;

int main() {
    unordered_map<string, int> hashTable; // unordered_map<key, value> 변수명;

    // 데이터 삽입
    hashTable["apple"] = 10;
    hashTable["banana"] = 20;

    // 데이터 검색
    if (hashTable.find("apple") != hashTable.end()) {
        cout << "Apple count: " << hashTable["apple"] << endl;
    }

    return 0;
}
```


****


## 💡 주요 메소드

|호출|설명| 
|:-------:|:-------:|
|intset()| 키 - 값 쌍 삽입|
|find()| 키 검색 |
|erase()| 특정 키 삭제|
|count()| 키가 존재하는지 확인|
|at()| 특정 키에 해당하는 값 반환|
|bucket()_count()| 버킷의 개수 반환|
|load_factor()| 요소 수 / 버킷 수 비율 반환|
|rehash()| 버킷 개수를 늘려 충돌 해결|



****


## 📊 시간 복잡도


||평균|최악|
|-----|-----|-----|
|삽입|\(O(1\))|\(O(n\))|
|검색|\(O(1\))|\(O(n\))|
|삭제|\(O(1\))|\(O(n\))|


****


## 📌 정리

- Hash table은 **빠른 데이터 검색** 을 해야할 때 유용하다.
- unordered_map 과 unordered_set 두 가지가 존재한다.
- unordered_map 은 **키-값(key-value)** 를 가지고 있다.
- unordered_set 은 **중복** 없는 **값** 을 가지고 있다.