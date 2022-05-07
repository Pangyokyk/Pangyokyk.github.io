Getting started with classification
==============

회귀 분석에 대한 이전 공부를 바탕으로 데이터를 더 잘 이해하는 데 사용할 수 있는 다른 분류기에 대해 알아보자.


1.Introduction to classification
------------
이 네 가지 lessons에서 classic machine learning 의 기본 초점인 Classification를 공부할 예정인데 
아시아와 인도의 모든 훌륭한 요리에 대한 데이터 세트를 사용하여 다양한 분류 알고리즘을 사용할 것이다.

분류는 회귀 기술과 많은 공통점이 있는 지도 학습의 한 형태이다.

머신 러닝이 데이터 세트를 사용하여 값이나 이름을 예측하는 것이라면 분류는 일반적으로 이진 분류와 다중 클래스 분류의 두 그룹으로 나뉜다.

복습:

- **Linear regression** 은 변수 간의 관계를 예측하고 해당 선과 관련하여 새 데이터 점이 포함될 위치를 정확하게 예측하는 데 도움이 된다.
- **Logistic regression** 은 "이진 범주"를 발견하는 데 도움을 준다.

분류는 다양한 알고리즘을 사용하여 데이터 포인트의 lable이나 클래스를 결정하는 다른 방법을 결정한다. 이 요리 데이터를 사용하여 재료 그룹을 관찰함으로써 음식의 유래를 결정할 수 있는지 알아보자.

* * *

### Introduction
분류는 머신러닝 연구자와 data 과학자의 기본 활동 중 하나이다. 이진 값의 기본 분류("이 이메일은 스팸입니까?")에서 컴퓨터 비전을 사용한 복잡한 이미지 분류 및 분할에 이르기까지 데이터를 클래스로 정렬하고 그에 대한 질문을 할 수 있으면 항상 유용하다.

과학적인 방법으로 공정을 설명하기 위해, 분류에서는 입력 변수와 출력 변수 사이의 관계를 매핑할 수 있는 예측 모형을 만든다.

![poster](../binary-multiclass.png)

spam 과 ham 을 구분하는 분류 알고리즘을 저번에 공부했었다.

데이터를 정리하고 시각화하고 ML 작업에 대비하는 프로세스를 시작하기 전에, 머신 러닝을 활용하여 데이터를 분류하는 다양한 방법에 대해 조금 알아보자.

통계에서 파생된 클래식 머신러닝을 사용한 분류는 흡연자, 체중 및 나이와 같은 특징을 사용하여 질병발생 가능성을 결정한다. 앞서 수행한 회귀 연습과 유사한 지도 학습 기법으로, 데이터에 레이블이 지정되고 ML 알고리즘은 이러한 label을 사용하여 데이터 세트의 클래스(또는 'feature')를 분류하고 예측하여 그룹 또는 결과에 할당한다.

* * *
## Hello 'classifier'

이 요리 데이터 세트에 대해 묻고 싶은 질문은 사실 multiclass 질문이다. 내가 할 수 있는 몇 가지 포텐셜한 국가 요리가 있기 때문이다. 성분 배치가 주어졌을 때, 이 많은 등급 중 어떤 데이터가 적합할지 알아야 할 것이다.

Scikit-learn은 해결하려는 문제의 종류에 따라 데이터를 분류하는 데 사용할 수 있는 몇 가지 다른 알고리즘을 제공한다. 이러한 알고리즘 중 몇 가지에 대해 공부해보자.

* * *
## Exercise - clean and balance your data
데이터를 정리하고 균형을 맞춰 더 나은 결과를 얻기 위해 데이터를 불러오자.

1. imblearn을 설치를 위해 다음과 같이 pip 설치를 실행
```python
pip install imblearn
```
2.데이터를 가져오는 데 필요한 패키지를 가져오고 시각화할 수 있으며, imblearn에서 SMOTE도 가져올 수 있다.
```python
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib as mpl
import numpy as np
from imblearn.over_sampling import SMOTE
```
3.데이터 불러오기
```python
df  = pd.read_csv('https://github.com/codingalzi/ML-For-Beginners/tree/main/4-Classification/data/cuisines.csv')
```
4. 데이터 확인
```python
df.head()
```
5.데이터 형태 확인
```output
    |     | Unnamed: 0 | cuisine | almond | angelica | anise | anise_seed | apple | apple_brandy | apricot | armagnac | ... | whiskey | white_bread | white_wine | whole_grain_wheat_flour | wine | wood | yam | yeast | yogurt | zucchini |
    | --- | ---------- | ------- | ------ | -------- | ----- | ---------- | ----- | ------------ | ------- | -------- | --- | ------- | ----------- | ---------- | ----------------------- | ---- | ---- | --- | ----- | ------ | -------- |
    | 0   | 65         | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
    | 1   | 66         | indian  | 1      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
    | 2   | 67         | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
    | 3   | 68         | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
    | 4   | 69         | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 1      | 0        |
```
6.데이터 info 확인
```python
df.info()
```

```output
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 2448 entries, 0 to 2447
Columns: 385 entries, Unnamed: 0 to zucchini
dtypes: int64(384), object(1)
memory usage: 7.2+ MB
```

* * *
## Exercise - learning about cuisines
