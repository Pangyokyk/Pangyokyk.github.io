---
layout: post
title: Hash table
tags: [STL, C++]
date: 2025-01-20 +0800
math: true
toc : true
---


# Hash table 정리


****


## Hash table이란?

- 데이터를 효율적으로 저장하고 검색하기 위해 사용하는 자료구조
- **<mark>키-값(key-value pair)</mark>** 쌍으로 데이터를 저장하며 내부적으로는 해시 함수를 이용해 키를 특정 위치에 매핑한다.
- C++ STL(Standard Template Library)에서는 주로 **unordered_map** 과 **unordered_set** 으로 해시테이블을 구현한다.

****


## 특징

**1. 빠른 검색**
   - 평균적으로 \(O(1\))의 시간 복잡도로 검색, 삽입, 삭제를 수행한다. 최악의 경우는 충돌이 많을 때 \(O(n\))이다. 

**2. 키- 값(key-value) 쌍으로 저장**

**3. 해시 함수**
  - 키를 특정 버킷으로 매핑하기 위해 사용된다. 해시함수가 충돌을 최소화 할수록 성능이 올라간다.
****

## 구현
```cpp
#include <iostream>
#include <string>
#include <unordered_map>

using namespace std;

int main()
{
    unordered_map<string, int> hash_table {
        {"apple", 5},
        {"banana", 3},
        {"pear", 10}
    };

    cout << hash_table["apple"] << endl;

    //모든 키값 출력해보기
    //출력하는 방법은 두가지가 존재


    // 1. range-based for loop
    for(const auto& pair : hash_table)
    {
        cout << pair.first << " : " << pair.second << endl;
    }

    // 2. iterator
    for(auto it = hash_table.begin(); it != hash_table.end(); it++)
    {
        cout << it->first << " : " << it->second << endl;
    }


    return 0;
}
```
****
## 코드 공부
```cpp
unordered_map<string, int> hash_table {
        {"apple", 5},
        {"banana", 3},
        {"pear", 10}
    };
```
- 해시 테이블을 적는 방법이다. 
- unordered_map<key, value> 함수명 { };
- 해시 테이블은 데이터를 입력한 순서대로 저장하지 않는다. 그래서 만약 전부 출력한다면 랜덤으로 정렬되어 출력된다.
  


```cpp
// 1. range-based for loop
    for(const auto& pair : hash_table)
    {
        cout << pair.first << " : " << pair.second << endl;
    }
```
- 범위 지정해 반복해서 해시 테이블 값들 전부 출력해보기
- **<mark>auto</mark>** 는 자동 타입 추론을 해주는 키워드이다. 변수 선언 시 초기화 값의 타입을 자동으로 추론하여 컴파일러가 결정한다
- 또한 &(reference)가 붙은 이유는 불필요한 메모리 복사를 막기 위함이다. 만약 hash_table의 크기가 매우 크다면 복사를 하는데 많은 메모리가 낭비되므로 프로그램이 저하된다. 그렇기 때문에 원본을 참조하도록 &(reference)를 붙여준다.


```cpp
// 2. iterator
    for(auto it = hash_table.begin(); it != hash_table.end(); it++)
    {
        cout << it->first << " : " << it->second << endl;
    }
```
- iterator 반복자를 통해 해시테이블 전부 뽑아보기



****

# 정리
c++ 에서 hash table 은 **unordered_map** 과 **unordered_set** 으로 구현할 수 있다. 빠른 검색, 삽입, 삭제를 제공하지만 충돌과 메모리 사용량 관리가 중요한 요소이며, 데이터 순서를 유지하려면 **map**을 사용해야한다.
(여기서는 unordered_map만 다뤘는데 unordered_set은 중복되지 않은 고유한 키만 저장하는 방법이다.)