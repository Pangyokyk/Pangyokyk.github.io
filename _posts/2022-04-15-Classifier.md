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

![binary-multiclass](https://user-images.githubusercontent.com/103716440/167249869-d50a3d59-ca7d-4ca9-a2bc-c6969f94aac3.png)

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

1. 데이터를 가져오는 데 필요한 패키지를 가져오고 시각화할 수 있으며, imblearn에서 SMOTE도 가져올 수 있다.

    ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import matplotlib as mpl
    import numpy as np
    from imblearn.over_sampling import SMOTE
    ```

1. 데이터 불러오기

    ```python
    df  = pd.read_csv('https://github.com/codingalzi/ML-For-Beginners/tree/main/4-Classification/data/cuisines.csv')
    ```

1. 데이터 모양 확인

    ```python
    df.head()
    ```

   맨앞 5개열 확인

    ```output
    |     | Unnamed: 0 | cuisine | almond | angelica | anise | anise_seed | apple | apple_brandy | apricot | armagnac | ... | whiskey | white_bread | white_wine | whole_grain_wheat_flour | wine | wood | yam | yeast | yogurt | zucchini |
    | --- | ---------- | ------- | ------ | -------- | ----- | ---------- | ----- | ------------ | ------- | -------- | --- | ------- | ----------- | ---------- | ----------------------- | ---- | ---- | --- | ----- | ------ | -------- |
    | 0   | 65         | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
    | 1   | 66         | indian  | 1      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
    | 2   | 67         | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
    | 3   | 68         | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
    | 4   | 69         | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 1      | 0        |
    ```

1. info로 데이터 정보 확인

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

요리 당 데이터의 분포 확인

1. 'barh()'를 호출하면 데이터를 막대로 보여준다.

    ```python
    df.cuisine.value_counts().plot.barh()
    ```

    ![cuisine-dist](https://user-images.githubusercontent.com/103716440/167250095-c9d9f430-12ff-499d-a694-8544d4bf3c8f.png)

    한정된 수의 요리가 있지만, 데이터의 분포가 고르지 않다.

1. 요리당 얼마나 많은 데이터를 사용할 수 있는지 확인하고 출력

    ```python
    thai_df = df[(df.cuisine == "thai")]
    japanese_df = df[(df.cuisine == "japanese")]
    chinese_df = df[(df.cuisine == "chinese")]
    indian_df = df[(df.cuisine == "indian")]
    korean_df = df[(df.cuisine == "korean")]
    
    print(f'thai df: {thai_df.shape}')
    print(f'japanese df: {japanese_df.shape}')
    print(f'chinese df: {chinese_df.shape}')
    print(f'indian df: {indian_df.shape}')
    print(f'korean df: {korean_df.shape}')
    ```

    출력은 다음과 같다.

    ```output
    thai df: (289, 385)
    japanese df: (320, 385)
    chinese df: (442, 385)
    indian df: (598, 385)
    korean df: (799, 385)
    ```

* * *
## Discovering ingredients
데이터를 더 깊이 파고들어 요리의 전형적인 재료가 무엇인지 알 수 있다.. 음식 사이에 혼란을 일으키는 반복적인 데이터를 지워야 하는데, 이 문제에 대해 알아보도록 하자.

1. Python에서 create_iningent() 함수를 만들어 성분 데이터 프레임을 생성한다. 도움이 되지 않는 열을 떨어뜨리고 성분 수를 기준으로 재료를 정렬

1. Python에서 create_iningent() 함수를 만들어 성분 데이터 프레임을 생성한다. 도움이 되지 않는 열을 떨어뜨리고 성분을 개수에 따라 정렬

    ```python
    def create_ingredient_df(df):
        ingredient_df = df.T.drop(['cuisine','Unnamed: 0']).sum(axis=1).to_frame('value')
        ingredient_df = ingredient_df[(ingredient_df.T != 0).any()]
        ingredient_df = ingredient_df.sort_values(by='value', ascending=False,
        inplace=False)
        return ingredient_df
    ```
 이제 요리별로 가장 인기 있는 10대 식재료에 대한 정보를 얻을 수 있다.
 
1. create_interent()를 호출하고 barh()를 호출하여 막대기로 표시:

    ```python
    thai_ingredient_df = create_ingredient_df(thai_df)
    thai_ingredient_df.head(10).plot.barh()
    ```

    ![thai](https://user-images.githubusercontent.com/103716440/167250031-ef8f7b1c-75f4-4f59-b600-18751c05345a.png)

1. 같은 방법으로 japanese도 표시

    ```python
    japanese_ingredient_df = create_ingredient_df(japanese_df)
    japanese_ingredient_df.head(10).plot.barh()
    ```

    ![japanese](https://user-images.githubusercontent.com/103716440/167250043-a1887d75-2a3e-4e85-915e-a01368b109d5.png)

1. chinese

    ```python
    chinese_ingredient_df = create_ingredient_df(chinese_df)
    chinese_ingredient_df.head(10).plot.barh()
    ```

    ![chinese](https://user-images.githubusercontent.com/103716440/167250051-a149cdd7-ee9b-4a73-aa3e-fc28c484abe9.png)

1. indian

    ```python
    indian_ingredient_df = create_ingredient_df(indian_df)
    indian_ingredient_df.head(10).plot.barh()
    ```

    ![indian](https://user-images.githubusercontent.com/103716440/167250061-87101337-088b-46c5-bab8-7320ca221823.png)

1. 마지막으로 korean

    ```python
    korean_ingredient_df = create_ingredient_df(korean_df)
    korean_ingredient_df.head(10).plot.barh()
    ```

    ![korean](https://user-images.githubusercontent.com/103716440/167250072-c66f5963-3a98-4963-9a51-c7195cd11048.png)

1. 다른 요리의 혼동을 일으키는 가장 흔한 재료를 'drop()'을 이용하여 없애자. 

    ```python
    feature_df= df.drop(['cuisine','Unnamed: 0','rice','garlic','ginger'], axis=1)
    labels_df = df.cuisine #.unique()
    feature_df.head()
    ```
    
* * *
## Balance the dataset
데이터를 정리했으니 [SMOTE](https://imbalanced-learn.org/dev/references/generated/imblearn.over_sampling.SMOTE.html) - "Synthetic Minority Over-sampling Technique" 를 사용해보자.

1. `fit_resample()` 호출, 이 방법은 보간법으로 새 샘플을 생성한다.

    ```python
    oversample = SMOTE()
    transformed_feature_df, transformed_label_df = oversample.fit_resample(feature_df, labels_df)
    ```

    데이터의 균형을 유지함으로써 데이터를 분류할 때 더 나은 결과를 얻을 수 있다. 이진 분류에 대해 생각해 보면 대부분의 데이터가 하나의 클래스인 경우 머신러닝 모델은 해당 클래스를 더 자주 예측한다. 그냥 더 많은 데이터가 있기 때문인데 데이터의 균형을 맞추려면 왜곡된 데이터가 필요하며 이러한 불균형을 제거하는 데 도움이 된다. 

1. 이제 성분당 label 수를 확인할 수 있습니다.

    ```python
    print(f'new label count: {transformed_label_df.value_counts()}')
    print(f'old label count: {df.cuisine.value_counts()}')
    ```
    
    output 확인

    ```output
    new label count: korean      799
    chinese     799
    indian      799
    japanese    799
    thai        799
    Name: cuisine, dtype: int64
    old label count: korean      799
    indian      598
    chinese     442
    japanese    320
    thai        289
    Name: cuisine, dtype: int64
    ```

    데이터가 깔끔하고 정리가 잘 되었다. 

1. 마지막 단계는 label 및 feature을 포함하여 균형 잡힌 데이터를 파일로 내보낼 수 있는 새 데이터 프레임에 저장하는 것이다.

    ```python
    transformed_df = pd.concat([transformed_label_df,transformed_feature_df],axis=1, join='outer')
    ```

1. 'transformed_df.head()'와 'transformed_df.info()'를 사용하여 데이터를 한 번 더 살펴볼 수 있다. 이 데이터의 복사본을 저장하여 향후에도 사용하자.

    ```python
    transformed_df.head()
    transformed_df.info()
    transformed_df.to_csv("https://github.com/codingalzi/ML-For-Beginners/tree/main/4-Classification/data/cleaned_cuisines.csv")
    ```

    이제 이 루트에서 사용가능하다.
    
    
* * *
# 2. Cuisine classifiers 1

이 과정에서는 위에서 저장한 모든 음식에 대한 균형이 잡히고 깔끔한 데이터로 가득 찬 데이터 세트를 사용한다.

이 데이터 세트를 다양한 분류기와 함께 사용하여 재료 그룹을 기반으로 특정 국가 음식을 예측한다. 이렇게 하는 동안 분류 작업에 알고리즘을 활용할 수 있는 몇 가지 방법에 대해 자세히 알아볼 수 있다.

## Exercise - predict a national cuisine

1. _notebook.ipynb_ 폴더에서 작업하면서 해당 파일을 Pandas 라이브러리와 함께 가져온다.:

    ```python
    import pandas as pd
    cuisines_df = pd.read_csv("https://github.com/codingalzi/ML-For-Beginners/tree/main/4-Classification/data/cleaned_cuisines.csv")
    cuisines_df.head()
    ```

    데이터는 이렇게 보인다.

|     | Unnamed: 0 | cuisine | almond | angelica | anise | anise_seed | apple | apple_brandy | apricot | armagnac | ... | whiskey | white_bread | white_wine | whole_grain_wheat_flour | wine | wood | yam | yeast | yogurt | zucchini |
| --- | ---------- | ------- | ------ | -------- | ----- | ---------- | ----- | ------------ | ------- | -------- | --- | ------- | ----------- | ---------- | ----------------------- | ---- | ---- | --- | ----- | ------ | -------- |
| 0   | 0          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 1   | 1          | indian  | 1      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 2   | 2          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 3   | 3          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 4   | 4          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 1      | 0        |


1. 몇가지 사이킷런 라이브러리를 불러오자.

    ```python
    from sklearn.linear_model import LogisticRegression
    from sklearn.model_selection import train_test_split, cross_val_score
    from sklearn.metrics import accuracy_score,precision_score,confusion_matrix,classification_report, precision_recall_curve
    from sklearn.svm import SVC
    import numpy as np
    ```

1. 훈련을 위해 X 및 Y 좌표를 두 개의 데이터 프레임으로 나눔. 'cuisine'은 label의 데이터 프레임이 될 수 있다.

    ```python
    cuisines_label_df = cuisines_df['cuisine']
    cuisines_label_df.head()
    ```

    cuisines_label 데이터 확인

    ```output
    0    indian
    1    indian
    2    indian
    3    indian
    4    indian
    Name: cuisine, dtype: object
    ```

1. 'Unnamed: 0' 열과 'cuisine' 열을 drop()을 이용하여 삭제한다. 나머지 데이터는 훈련가능한 데이터이므로 남겨둔다.

    ```python
    cuisines_feature_df = cuisines_df.drop(['Unnamed: 0', 'cuisine'], axis=1)
    cuisines_feature_df.head()
    ```

    feature 확인

|      | almond | angelica | anise | anise_seed | apple | apple_brandy | apricot | armagnac | artemisia | artichoke |  ... | whiskey | white_bread | white_wine | whole_grain_wheat_flour | wine | wood |  yam | yeast | yogurt | zucchini |
| ---: | -----: | -------: | ----: | ---------: | ----: | -----------: | ------: | -------: | --------: | --------: | ---: | ------: | ----------: | ---------: | ----------------------: | ---: | ---: | ---: | ----: | -----: | -------: |
|    0 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    1 |      1 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    2 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    3 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    4 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      1 |        0 | 0 |

훈련준비가 완료되었다.

* * *
## 분류기 고르기

이제 데이터를 깔끔하게 조정했고 훈련 준비가 되었으므로 작업에 사용할 알고리즘을 결정해야 한다. 

Scikit-learn 그룹에서 "지도 학습"에서 분류되며, 이 범주에서 분류할 수 있는 여러 가지 방법을 찾을 수 있다 [The variety](https://scikit-learn.org/stable/supervised_learning.html) 여기서 확인 가능하다. 다음 방법에는 모두 분류 기법이 포함된다.

- Linear Models
- Support Vector Machines
- Stochastic Gradient Descent
- Nearest Neighbors
- Gaussian Processes
- Decision Trees
- Ensemble methods (voting Classifier)
- Multiclass and multioutput algorithms (multiclass and multilabel classification, multiclass-multioutput classification)

### 무슨 분류를 사용해야할까?
어떤 분류기를 선택해야 할까? 여러 개를 훑어보고 좋은 결과를 찾는 것이 테스트하는 방법일 것이다. Scikit-learn은 생성된 데이터 세트에 대해 KNeigbors, SVC의 두 가지 방법, GaussianProcessClassifier, DecisionTreeClassifier, RandomForestClassifier, MLPClassifier, AdaBoostClassifier, GaussianNB, QuadraticDiscrinationAnalysis의 시각화로 나타낸 결과를 보자.



### 좀 더 좋은 접근법
하지만 막 추측하는 것보다 더 나은 방법은 다운로드 가능한 '머신러닝 cheat sheet'의 아이디어를 따르는 것이다. 여기서, 다중 클래스 문제에 대해 몇 가지 선택사항이 있다는 것을 알 수 있다.



### 추론
내가 가지고 있는 제약조건에 따라 다른 접근방식을 통해 길을 추론할 수 있는지 보자.

- **Neural networks 은 너무 무겁다.**. 깔끔하지만 최소한의 데이터 세트와 노트북을 통해 local로 훈련을 실행하고 있다는 사실을 감안할 때 **Neuralnetworks**은 이 작업에 너무 무겁다.
- **two-class classifier 사용하지않음**. 우리는 two-class classifier를 사용하지 않기 때문에, 그것은 One-VS-All을 배제한다. 
- **결정트리와 or 로지스틱회귀 사용가능**. 결정 트리가 작동하거나 다중 클래스 데이터에 대해 로지스틱 회귀 분석을 사용할 수 있다.

### Scikit-learn 사용하기
Scikit-learn을 사용하여 데이터를 분석할 것이다. Scikit-learn에서는 로지스틱 회귀 분석을 사용하는 여러 가지 방법이 있는데 [parameters to pass](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html?highlight=logistic%20regressio#sklearn.linear_model.LogisticRegression) 여기서 확인가능하다.

기본적으로 Scikit-learn에게 로지스틱 회귀 분석을 수행하도록 요청할 때 지정해야 하는 multi_class와 solver라는 두 가지 중요한 매개 변수가 있다. multi_class 값은 특정 동작을 적용하고 solver의 값은 사용할 알고리즘입니다. 그렇다고 모든 solver를 모든 multi_class 값과 쌍으로 구성할 수 있는 것은 아니다

문서에 따르면, multiclass 사례에서 훈련 알고리즘은 다음과 같다.
- **one-vs-rest (OvR) 사용**, 'multi_class' 옵션을 'ovr'로 설정하면 된다.
- **cross-entropy loss 사용**, 'multi_class' 옵션을 'multinatic'으로 설정하면 된다. (현재 'multinomial' 옵션은 'lbfgs', 'sag', 'saga', 'newton-cg' solvers에서만 지원)"

Scikit-learn에서 solvers가 다양한 종류의 데이터 구조에서 나타나는 다양한 문제를 처리하는 방법을 표로 정리하면 이렇다.



## Exercise - data 나누기
train_test_split()를 호출하여 데이터를 훈련 및 테스트 그룹으로 나누자.

```python
X_train, X_test, y_train, y_test = train_test_split(cuisines_feature_df, cuisines_label_df, test_size=0.3)
```

## Exercise - 로지스틱 회귀 적용
다중클래스 case를 사용 중이므로 사용할 scheme과 설정할 solver를 선택해야 한다. 다중 클래스 설정 및 liblinear solver와 함께 로지스틱 회귀 분석을 사용하여 훈련한다.

1. multi_class를 'ovr'로 설정하고 solver를 'liblinear'로 설정하여 로지스틱 회귀 분석 생성

    ```python
    lr = LogisticRegression(multi_class='ovr',solver='liblinear')
    model = lr.fit(X_train, np.ravel(y_train))
    
    accuracy = model.score(X_test, y_test)
    print ("Accuracy is {}".format(accuracy))
    ```
    정확도가 80프로 이상 나온다.
    
    1. 데이터 행 하나를 테스트하면 이 모델이 작동하는 것을 볼 수 있다.

    ```python
    print(f'ingredients: {X_test.iloc[50][X_test.iloc[50]!=0].keys()}')
    print(f'cuisine: {y_test.iloc[50]}')
    ```

    결과를 확인해 보자.

   ```output
   ingredients: Index(['cilantro', 'onion', 'pea', 'potato', 'tomato', 'vegetable_oil'], dtype='object')
   cuisine: indian
   ```
   
   1. 더 깊이 파고들면 이 예측의 정확성을 확인할 수 있다.

    ```python
    test= X_test.iloc[50].values.reshape(-1, 1).T
    proba = model.predict_proba(test)
    classes = model.classes_
    resultdf = pd.DataFrame(data=proba, columns=classes)
    
    topPrediction = resultdf.T.sort_values(by=[0], ascending = [False])
    topPrediction.head()
    ```

    결과를 확인해보면 인도 요리가 가장 잘 추측되는걸 알 수 있다.

    |          |        0 |
    | -------: | -------: |
    |   indian | 0.715851 |
    |  chinese | 0.229475 |
    | japanese | 0.029763 |
    |   korean | 0.017277 |
    |     thai | 0.007634 |
    
    1. 정밀도 재현율 f1-score를 확인하여 좀 더 정확하게 확인해보자.

    ```python
    y_pred = model.predict(X_test)
    print(classification_report(y_test,y_pred))
    ```

    |              | precision | recall | f1-score | support |
    | ------------ | --------- | ------ | -------- | ------- |
    | chinese      | 0.73      | 0.71   | 0.72     | 229     |
    | indian       | 0.91      | 0.93   | 0.92     | 254     |
    | japanese     | 0.70      | 0.75   | 0.72     | 220     |
    | korean       | 0.86      | 0.76   | 0.81     | 242     |
    | thai         | 0.79      | 0.85   | 0.82     | 254     |
    | accuracy     | 0.80      | 1199   |          |         |
    | macro avg    | 0.80      | 0.80   | 0.80     | 1199    |
    | weighted avg | 0.80      | 0.80   | 0.80     | 1199    |
