---
layout: post
title: 📚 KMP 알고리즘
tags: [C++, KMP, string]
date: 2025-05-31 +0800
math: true
toc : true
---




# 📚 KMP 알고리즘 
- 문자열 검색 알고리즘
- 텍스트 내에서 패턴 문자열을 빠르게 찾는 알고리즘
- **일반적인 문자열 탐색(브루트포스)이 \(O(n*m\)) 인 반면 KMP는 \(O(n+m\)) 시간에 수행한다.(n = 텍스트 길이, m = 패턴 길이)**
- [KMP를 잘 정리해준 사이트](https://10000cow.tistory.com/entry/%EB%AC%B8%EC%9E%90%EC%97%B4-%EA%B2%80%EC%83%89-%ED%95%9C-%EC%82%B4%EB%8F%84-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-KMP-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)

## 🔎 KMP 핵심 아이디어 
- 패턴 내에서 실패할 때 다시 처음부터 비교하지 않고, **이미 일치했던 부분을 활용해** 건너뛴다.
- 이를 위해 **접두사-접미사 일치 정보** 를 미리 계산하는 과정이 필요하다.
- 이를 lps[] 배열이라 부른다(Longest Prefix Seffix)



## 🚀 KMP 단계별 설명 

1. **lps 배열 반들기**
   1. lps[i]는 패턴의 0 ~ i 까지 부분 문자열에서 접두사와 접미사가 일치하는 가장 긴 길이를 뜻한다.
   2. ex) 패턴 : "ABABC", lps 배열 : [0, 0, 1, 2, 0]

2. **문자열 탐색**
   1. 텍스트와 패턴을 순회하며 일치 여부 검사
   2. 불일치가 발생하면 패턴에서 바로 이전 lps값을 참조해 비교를 계속함으로써 불필요한 반복 방지



## 💻 KMP 구현


```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

// 1. lps 배열 계산 함수
vector<int> computeLps(const string& pattern) {
    int m = pattern.size();
    vector<int> lps(m, 0);

    int len = 0; // 현재까지의 접두사 - 접미사 최대 길이
    int i = 1; // lps[0]은 항상 0이므로 i = 1부터 시작

    while(i < m)
    {
        if(pattern[i] == patter[len])
        {
            len++;
            lps[i] = len;
            i++;
        }
        else
        {
            if(len != 0)
            {
                len = lps[len-1]; // len을 lps[len-1]로 줄여서 다시 비교
            }
            else
            {
                lps[i] = 0;
                i++;
            }
        }
    }return lps;
}


// 2. KMP 검색 함수 : 패턴이 텍스트에 있는지 확인
bool KMP(const string& text, const string& pattern) {
    int n = text.size();
    int m = pattern.size();

    if(m == 0)  return true; // 빈 패턴은 항상 존재함
    
    vector<int> lps = coputeLps(pattern);

    int i = 0; // 텍스트 인덱스
    int j = 0; // 패턴 인덱스

    while(i < n)
    {
        if(text[i] == pattern[j])
        {
            i++;
            j++;

            if(j == m)
            {
                // 패턴 전부 일치
                return true;
                // j = lps[j-1]; // 여러개 찾으려면 여기서 계속 진행
            }
        }
        else
        {
            if(j != 0)
            {
                j = lps[j-1];
            }
            else
            {
                i++;
            }
        }
    }
    return false; // 패턴없음
}

```



## 📌 요약 
- KMP 알고리즘 작성에는 2가지 단계가 존재한다.
1. computeLps - 패턴에서 접두사-접미사 일치 길이 계산
2. KMP - 텍스트 내에서 패턴 검색

- lps 배열은 불일치 시 이동할 위치를 미리 계산해 시간 낭비를 방지한다.
- **전체 탐색이 \(O(n+m\)) 에 수행된다.**