---
layout: post
title: Unordered_map 정리
tags: [STL, C++, hash table]
date: 2025-01-20 +0800
math: true
toc : true
---

# unordered_map 정리

****

## unordered_map이란?

- C++ 표준 라이브러리에서 제공하는 해시 기반의 연관 컨테이너이다.

- 이 컨테이너는 키-값(key-value pair)을 저장하며, 키를 기반으로 빠르게 값을 검색할 수 있도록 설계되었다.
- map과 비슷하지만, 내부적으로 해시테이블을 사용하기 때문에 <mark>**정렬되지 않은 상태로 데이터를 저장하며 탐색속도가 평균적으로 더 빠르다.**<mark>


****



## 주요 특징
1. <mark>**해시테이블 기반**</mark>
   - 해시테이블을 사용하여 데이터를 저장한다.
   - 키에 대해 해시 함수를 사용하여 저장 위치를 결정한다.
   - 평균 시간 복잡도는 삽입, 삭제, 검색 모두 \(O(1)\) 이지만 충돌이 많아 최악의 경우 \(O(n)\)이다.

2. <mark> **정렬되지 않음** <mark>
   - unordered_map은 데이터가 입력된 순서나 키 순서로 정렬되지 않음
   - 정렬이 필요하다면 <mark>map</mark>를 사용
  
3. <mark>**키의 고유성**</mark>
    - 각 키는 고유해야하며, 동일한 키를 가진 여러 항목을 저장할 수 없다.
    - 동일한 키를 삽입하려면 기존의 키의 값이 새값으로 대체된다.
  
4. <mark>**사용자 정의키와 해시 함수 지원**</mark>
   - 기본적으로 unordered_map은 hash를 사용하여 해시값을 계산한다.
   - 사용자 정의 키 타입을 사용하거나 기본 해시 함수를 대체하려면 사용자 정의 함수와 비교 연산자를 제공해야한다.

****
## 예시
```cpp
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    // unordered_map 선언
    std::unordered_map<std::string, int> ageMap;

    // 삽입
    ageMap["Alice"] = 25;
    ageMap["Bob"] = 30;
    ageMap["Charlie"] = 35;

    // 접근
    std::cout << "Alice's age: " << ageMap["Alice"] << "\n";

    // 검색
    auto it = ageMap.find("Bob");
    if (it != ageMap.end()) {
        std::cout << "Bob's age: " << it->second << "\n";
    } else {
        std::cout << "Bob not found.\n";
    }

    // 삭제
    ageMap.erase("Charlie");

    // 순회 (순서는 보장되지 않음)
    for (const auto& pair : ageMap) {
        std::cout << pair.first << " is " << pair.second << " years old.\n";
    }

    return 0;
}
```
****
## 주요 함수

   - **삽입 및 수정**
   ```cpp
   mymap[key] = value; // 키가 없으면 새로 삽입, 있으면 값 수정
   mymap.insert({key, value}); // 명시적 삽입
   ```

   - **삭제**
  ```cpp
   mymap.erase(key); // 특정 키 삭제
   mymap.clear(); // 모든 요소 삭제
   ```

   - **검색**
  ```cpp
  mymap.find(key); // 키를 가진 요소를 찾고 iterator반환
  mymap.count(key); // 키 존재 여부를 확인(없으면 0 있으면 1 반환)
  ```

  - **크기 확인**
  ```cpp
  mymap.size(); //요소 개수 반환
  mymap.empty(); // 비어 있는지 확인
  ```
****
## 사용자 정의 키와 해시 함수 예시 
```cpp

#include <iostream>
#include <unordered_map>

struct Point {
    int x, y;

    // 비교를 위해 == 연산자 정의
    bool operator==(const Point& other) const {
        return x == other.x && y == other.y;
    }
};

// 사용자 정의 해시 함수
struct PointHash {
    size_t operator()(const Point& p) const {
        return std::hash<int>()(p.x) ^ std::hash<int>()(p.y);
    }
};

int main() {
    std::unordered_map<Point, std::string, PointHash> pointMap;

    pointMap[{1, 2}] = "A";
    pointMap[{3, 4}] = "B";

    for (const auto& pair : pointMap) {
        std::cout << "(" << pair.first.x << ", " << pair.first.y << ") -> " << pair.second << "\n";
    }

    return 0;
}
```


|특징|unordered_map|map|
|---|----------------------|-----|
|기본 구조| 해시 테이블 | 이진 검색 트리|
|정렬| 데이터 정렬되지 않음 | 키 기준으로 정렬|
시간 복잡도| 평균 \(O(1\)) 최악의 경우 \(O(n\))|\(O(log(n\)))|
|사용사례| 빠른 검색, 삽입, 삭제| 정렬된 데이터가 필요할 때|


****


## 후기
Leetcode에서 문제를 고르고 풀고 공부 후 거기에 쓰이는 문법과 알고리즘에 대해 공부하는 방식으로 실행하고 있다.
