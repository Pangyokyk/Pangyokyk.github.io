---
layout: post
title: List
tags: [STL, C++, binary search]
date: 2025-01-22 +0800
math: true
toc : true
---



# 이진 탐색(Binary search)


****


## 이진 탐색이란?
- 정렬된 배열이나 리스트에서 특정 값을 효율적으로 찾는 알고리즘이다.
- 이진 탐색은 반으로 나누어가며 검색을 진행하기 때문에 시간 복잡도가 매우 빠르다\(O(logn\))의 시간 복잡도, 즉 **<mark>배열의 크기가 커져도 탐색 횟수가 급격히 증가하지 않음</mark>** 을 의미한다.


****


## 이진 탐색의 작동 원리
1. 배열 정렬
   - 이진 탐색은 <mark>**반드시 정렬된 배열**</mark> 에서만 작동한다

2. 중앙값 계산
   - 배열의 중앙에 있는 값을 확인한다. 그 값이 찾고자 하는 값이라면 탐색 종료

3. 값 비교
   - 찾고자하는 값이 중앙값보다 크면, 탐색 범위를 중앙값의 오른쪽 절반으로 줄임
   - 찾고자하는 값이 중앙값보다 작으면, 탐색 범위를 중앙값의 왼쪽 절반으로 줄임

4. 재귀적 혹은 반복적 탐색
   - 배열의 범위가 좁아질 때까지 이 과정을 반복, 더 이상 찾을 값이 없으면, 찾지 못했다는 결과를 반환(보통 -1)


****


## 이진 탐색 예시

```cpp
#include <iostream>

using namespace std;


int binary_search(const vector<int>& arr, int target) {
    int low = 0;
    int high = arr.size() - 1; // index의 크기

    while(low<=high)
    {
        int mid = low + (high - low) / 2 //중간값 계산(오버플로우 방지)

        if(arr[mid] == target)
        {
            return mid; // 중앙값과 찾고자 하는 값이 같으면 mid를 리턴
        }
        else if(arr[mid] <  target)
        {
            low = mid + 1; // 배열의 중앙값이 찾고자 하는 값보다 작으면 더 오른쪽에 있다는 뜻이니깐 탐색 범위를 오른쪽으로 좁힌다.
        }
        else
        {
            high = mid - 1; // 배열의 중앙값이 찾고자 하는 값보다 크면 왼쪽에 있다는 뜻이니깐 탐색 범위를 왼쪽으로 좁힌다.
        }
    }

    return -1; //찾지 못한다면 -1 리턴

}

int main()
{

    vector<int> arr = {1, 3, 5, 7, 9, 10};

    int target = 7;
    int result = binary_search(arr, target);

    if(result == -1)
    {
        cout << "찾지 못함!" << endl;
    }
    else
    {
        cout << "찾음!" << result << endl;
    }

    return 0;
}

```


****


## 이진 탐색의 재귀적 구현

```cpp
#include <iostream>
#include <vector>

int binarySearchRecursive(const std::vector<int>& arr, int target, int low, int high) {
    if (low > high) return -1;  // 값을 찾지 못했을 경우

    int mid = low + (high - low) / 2;

    if (arr[mid] == target) {
        return mid;  // 값이 중간값과 일치하면 그 인덱스를 반환
    } else if (arr[mid] < target) {
        return binarySearchRecursive(arr, target, mid + 1, high);  // 오른쪽 절반으로
    } else {
        return binarySearchRecursive(arr, target, low, mid - 1);  // 왼쪽 절반으로
    }
}

int main() {
    std::vector<int> arr = {1, 3, 5, 7, 9, 11, 13, 15};

    int target = 7;
    int result = binarySearchRecursive(arr, target, 0, arr.size() - 1);

    if (result != -1) {
        std::cout << "Element found at index: " << result << std::endl;
    } else {
        std::cout << "Element not found" << std::endl;
    }

    return 0;
}
```



****


## 이진 탐색의 주요특징
1. 시간 복잡도
   - \(O(logn\)) 로, 탐색 범위가 절반씩 줄어들기 때문에 매우 빠르다.

2. 공간 복잡도
   - \(O(1\)) (반복문 사용시), 재귀 호출을 사용하면 호출 스택이 필요하므로 \(O(logn\)) 이 될 수도 있다.

3. **<mark>정렬된 배열에서만 동작</mark>**
   - binary search 에서 가장 중요한 이론
   - **<mark>이진 탐색을 사용하려면 배열이나 리스트가 반드시 정렬되어 있어야한다.</mark>**
   - 만약 정렬되어 있지 않다면, 먼저 정렬을 해야한다.

4. 정확한 값을 찾지 못햇을 경우
   - 값을 찾지 못했을 경우 일반적으로 '-1' 을 반환한다. 또는 nullptr(포인터일 경우)를 반환한다.



****


## 정리
이진 탐색은 코딩 테스트에 꼭 필요한 개념이다. 탐색 속도가 빠른만큼 **무조건 정렬된 데이터** 에서만 사용 가능이라는 조건이 붙어있다. 