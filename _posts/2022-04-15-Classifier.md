Getting started with classification
==============

회귀 분석에 대한 이전 공부를 바탕으로 데이터를 더 잘 이해하는 데 사용할 수 있는 다른 분류기에 대해 알아보자.


# 1.Introduction to classification
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

[spam 과 ham 을 구분하는 분류 알고리즘을 저번에 공부했었다.]

데이터를 정리하고 시각화하고 ML 작업에 대비하는 프로세스를 시작하기 전에, 머신 러닝을 활용하여 데이터를 분류하는 다양한 방법에 대해 조금 알아보자.

통계에서 파생된 클래식 머신러닝을 사용한 분류는 흡연자, 체중 및 나이와 같은 특징을 사용하여 질병발생 가능성을 결정한다. 앞서 수행한 회귀 연습과 유사한 지도 학습 기법으로, 데이터에 레이블이 지정되고 ML 알고리즘은 이러한 label을 사용하여 데이터 세트의 클래스(또는 'feature')를 분류하고 예측하여 그룹 또는 결과에 할당한다.

* * *
## 'classifier'

이 요리 데이터 세트에 대해 묻고 싶은 질문은 사실 multiclass 질문이다. 내가 할 수 있는 몇 가지 포텐셜한 국가 요리가 있기 때문이다. 성분 배치가 주어졌을 때, 이 많은 등급 중 어떤 데이터가 적합할지 알아야 할 것이다.

Scikit-learn은 해결하려는 문제의 종류에 따라 데이터를 분류하는 데 사용할 수 있는 몇 가지 다른 알고리즘을 제공한다. 이러한 알고리즘 중 몇 가지에 대해 공부해보자.

* * *
## Exercise - data를 균형있고 깔끔하게 
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
## Exercise - cuisines 데이터 

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
## 성분 
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
## 데이터셋 균형맞추기
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

## Exercise - national cuisine 예측

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

![comparison](https://user-images.githubusercontent.com/103716440/167251534-ab9b670b-c7b4-4f9b-bd39-be6c7fd96fac.png)

### 좀 더 좋은 접근법
하지만 막 추측하는 것보다 더 나은 방법은 다운로드 가능한 '머신러닝 cheat sheet'의 아이디어를 따르는 것이다. 여기서, 다중 클래스 문제에 대해 몇 가지 선택사항이 있다는 것을 알 수 있다.

<img width="364" alt="cheatsheet" src="https://user-images.githubusercontent.com/103716440/167251550-3a546c71-5f77-47a8-835e-240740e7f747.png">


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

<img width="765" alt="solvers" src="https://user-images.githubusercontent.com/103716440/167251571-2e24c066-b8c7-4f84-9058-807cd1baee60.png">


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
    
    
    
# 3. Cuisine classifiers 2
이번에는 숫자 데이터를 분류하는 더 많은 방법을 살펴보자. 또한 한 분류기를 다른 분류기로 선택하는데 미치는 영향에 대해서도 알 수 있다.

## 분류 MAP
이전에는 Microsoft의 cheat sheet를 사용하여 데이터를 분류할 때 사용할 수 있는 다양한 옵션에 대해 배웠다. Scikit-learn은 추정기를 더 좁히는 데 도움이 되고 유사하지만 보다 세분화된 cheat sheet를 제공한다.

![map](https://user-images.githubusercontent.com/103716440/167252371-42b1cb22-5423-4f83-9d19-fb8e207e3a32.png)

### 계획
이 지도는 데이터를 명확하게 파악하면 의사 결정에 이르는 경로를 보면서 따라갈 수 있으므로 매우 유용하다.
한번 예시를 들어 지도를 따라가보자.

- 50개의 샘플을 가지고 있음.
- 범주를 예측하고 싶음.
- label 데이터를 가지고 있음.
- 10만개 미만의 샘플보유.
- 그렇다면 Linear SVC를 선택할 수 있다.
- 만약 안된다 하더라도 수치 데이터를 가지고 있기 때문에 KNeighbors Classifier를 사용할 수 있다.

## Exercise - data 나누기
라이브러리 가져오기

1. 필요한 라이브러리

    ```python
    from sklearn.neighbors import KNeighborsClassifier
    from sklearn.linear_model import LogisticRegression
    from sklearn.svm import SVC
    from sklearn.ensemble import RandomForestClassifier, AdaBoostClassifier
    from sklearn.model_selection import train_test_split, cross_val_score
    from sklearn.metrics import accuracy_score,precision_score,confusion_matrix,classification_report, precision_recall_curve
    import numpy as np
    ```

1. 훈련셋과 테스트셋 나누기

    ```python
    X_train, X_test, y_train, y_test = train_test_split(cuisines_feature_df, cuisines_label_df, test_size=0.3)
    ```
    
## Linear SVC classifier
SVC(Support-Vector clustering)는 ML 기술인 Support-Vector machine에 포함되어 있다. 이 방법에서는 'kernel'을 선택하여 label을 군집화하는 방법을 결정할 수 있다.

'C' 파라미터는 파라미터의 영향을 조절하는 '정규화'를 의미한다. 커널은 여러 가지 중 하나인데 여기서는 선형 SVC를 활용하도록 'linear'으로 설정한다. 그리고 확률은 기본적으로 'false'로 설정되며, 여기서는 확률 추정치를 수집하기 위해 'true'로 설정한다. 확률값을 얻어야 하기 때문에 데이터를 섞어 무작위 상태를 '0'으로 설정했다.

### Exercise - linear SVC 적용
분류 배열을 만드는 것으로 시작, 테스트하는 대로 이 어레이에 점진적으로 추가한다.

1. Linear SVC 시작

    ```python
    C = 10
    # Create different classifiers.
    classifiers = {
        'Linear SVC': SVC(kernel='linear', C=C, probability=True,random_state=0)
    }
    ```

2. Linear SVC을 모델에 훈련시키고 결과를 출력

    ```python
    n_classifiers = len(classifiers)
    
    for index, (name, classifier) in enumerate(classifiers.items()):
        classifier.fit(X_train, np.ravel(y_train))
    
        y_pred = classifier.predict(X_test)
        accuracy = accuracy_score(y_test, y_pred)
        print("Accuracy (train) for %s: %0.1f%% " % (name, accuracy * 100))
        print(classification_report(y_test,y_pred))
    ```

    Linear SVC의 정확도는 78.6% 정밀도 재현율 f1 점수도 확인 가능하다.

    ```output
    Accuracy (train) for Linear SVC: 78.6% 
                  precision    recall  f1-score   support
    
         chinese       0.71      0.67      0.69       242
          indian       0.88      0.86      0.87       234
        japanese       0.79      0.74      0.76       254
          korean       0.85      0.81      0.83       242
            thai       0.71      0.86      0.78       227
    
        accuracy                           0.79      1199
       macro avg       0.79      0.79      0.79      1199
    weighted avg       0.79      0.79      0.79      1199
    ```


## K-Neighbors classifier
K-Neighbors는 ML 방법의 "Neighbors" 계열의 일부로, 지도 학습과 비지도 학습 모두에 사용할 수 있다. 이 방법에서는 데이터에 대한 일반화된 label을 예측할 수 있도록 미리 정의된 수의 v포인트가 생성되고 이러한 포인트 주위의 데이터가 수집된다.


### Exercise - K-Neighbors classifier 적용
이전의 분류기는 좋았고, 데이터도 잘 작동했지만, 정확도 점수가 그렇게 좋지 못했다. K-Neighbors 분류기를 사용해 보자.

1. 분류 배열에 줄 추가(Linear SVC 항목 뒤에 쉼표 추가):

    ```python
    'KNN classifier': KNeighborsClassifier(C),
    ```

    결과가 아까보다 나쁘다.

    ```output
    Accuracy (train) for KNN classifier: 73.8% 
                  precision    recall  f1-score   support
    
         chinese       0.64      0.67      0.66       242
          indian       0.86      0.78      0.82       234
        japanese       0.66      0.83      0.74       254
          korean       0.94      0.58      0.72       242
            thai       0.71      0.82      0.76       227
    
        accuracy                           0.74      1199
       macro avg       0.76      0.74      0.74      1199
    weighted avg       0.76      0.74      0.74      1199
    ```
    

## Support Vector Classifier
Support-Vector Classifier는 분류 및 회귀 작업에 사용되는 ML 메서드의 Support-Vector Machine의 일부입니다. SVM은 두 범주 간의 거리를 최대화하기 위해 "공간 내 지점에 훈련 예제를 매핑"한다. 후속 데이터는 해당 범주를 예측할 수 있도록 이 공간에 매핑된다.


### Exercise - Support Vector Classifier 적용
Support Vector Classifier를 사용하여 더 나은 정확도를 뽑아보자.

1. K-Neighbors 항목 뒤에 쉼표를 추가하고 다음 줄을 추가.

    ```python
    'SVC': SVC(),
    ```

    정확도가 좀 더 올랐다.

    ```output
    Accuracy (train) for SVC: 83.2% 
                  precision    recall  f1-score   support
    
         chinese       0.79      0.74      0.76       242
          indian       0.88      0.90      0.89       234
        japanese       0.87      0.81      0.84       254
          korean       0.91      0.82      0.86       242
            thai       0.74      0.90      0.81       227
    
        accuracy                           0.83      1199
       macro avg       0.84      0.83      0.83      1199
    weighted avg       0.84      0.83      0.83      1199
    ```


## Classifiers 모음
다른 Classifiers을 사용해보자. 그중 랜덤 포레스트와 AdaBoost를 사용해보자.

```python
  'RFST': RandomForestClassifier(n_estimators=100),
  'ADA': AdaBoostClassifier(n_estimators=100)
```

랜덤 포레스트의 경우 결과가 매우 우수하다.

```output
Accuracy (train) for RFST: 84.5% 
              precision    recall  f1-score   support
     chinese       0.80      0.77      0.78       242
      indian       0.89      0.92      0.90       234
    japanese       0.86      0.84      0.85       254
      korean       0.88      0.83      0.85       242
        thai       0.80      0.87      0.83       227
    accuracy                           0.84      1199
   macro avg       0.85      0.85      0.84      1199
weighted avg       0.85      0.84      0.84      1199
Accuracy (train) for ADA: 72.4% 
              precision    recall  f1-score   support
     chinese       0.64      0.49      0.56       242
      indian       0.91      0.83      0.87       234
    japanese       0.68      0.69      0.69       254
      korean       0.73      0.79      0.76       242
        thai       0.67      0.83      0.74       227
    accuracy                           0.72      1199
   macro avg       0.73      0.73      0.72      1199
weighted avg       0.73      0.72      0.72      1199
```

이 머신러닝은 모델의 품질을 향상시키기 위해 여러 기본 추정기의 예측을 결합한다. 이 예에서는 랜덤 트리와 AdaBoost를 사용했다.


# 4. Appliied

## 요리 추천 웹 앱 구축
지금까지 앞에서 배운것들을 활용하여 맛있는 요리 데이터 세트를 사용하여 분류 모델을 구축한다. 또한, Onnx의 웹 런타임을 활용하여 저장된 모델을 사용할 수 있는 작은 웹 앱을 만들수 있다. 머신러닝의 활용으로 이런 것을 만들 수 있는데 난 아직 배우지 않았기 때문에 글을 읽으며 이해해 보자...

## 나의 모델 구축
응용 ML 시스템을 구축하는 것은 비즈니스 시스템에 이러한 기술을 활용하는 데 있어 중요한 부분이다. Onnx를 사용하여 웹 응용 프로그램 내에서 모델을 사용할 수 있으므로 필요한 경우 오프라인 컨텍스트에서 모델을 사용할 수 있다. Onnx의 기본 모델을 사용할 수도 있고 내가 모델을 만들 수도 있나보다.

이 과정에서는 추론을 위한 기본 JavaScript 기반 시스템을 구축할 수 있는데 먼저 모델을 교육하고 Onnx에서 사용하도록 변환해야 한다.

## Exercise - classification model 훈련

먼저 우리가 사용한 깔끔한 요리 데이터 세트를 사용하여 분류 모델을 훈련한다.

1. 사용할 라이브러리 불러오기

    ```python
    !pip install skl2onnx
    import pandas as pd 
    ```

    Scikit-learn 모델을 Onnx 형식으로 변환하려면 '[skl2onnx](https://onnx.ai/sklearn-onnx/)'가 필요하다.

1. 그런 다음 'read_csv()'를 사용하여 CSV 파일을 읽어 이전과 동일한 방식으로 데이터를 처리

    ```python
    data = pd.read_csv('https://github.com/codingalzi/ML-For-Beginners/tree/main/4-Classification/data/cleaned_cuisines.csv')
    data.head()
    ```

1. 처음 두 개의 불필요한 열을 제거하고 나머지 데이터를 'X'로 저장

    ```python
    X = data.iloc[:,2:]
    X.head()
    ```

1. label을 'y'로 저장

    ```python
    y = data[['cuisine']]
    y.head()
    
    ```

### 훈련 루틴을 시작
정확도가 좋은 'SVC' 라이브러리를 사용

1. 사이킷런으로부터 사용할 라이브러리 불러오기

    ```python
    from sklearn.model_selection import train_test_split
    from sklearn.svm import SVC
    from sklearn.model_selection import cross_val_score
    from sklearn.metrics import accuracy_score,precision_score,confusion_matrix,classification_report
    ```

1. 훈련셋과 테스트셋 나누기

    ```python
    X_train, X_test, y_train, y_test = train_test_split(X,y,test_size=0.3)
    ```

1. SVC Classification 모델을 구축

    ```python
    model = SVC(kernel='linear', C=10, probability=True,random_state=0)
    model.fit(X_train,y_train.values.ravel())
    ```

1. `predict()`을 이용하여 모델 평가하기

    ```python
    y_pred = model.predict(X_test)
    ```

1. classification_report을 사용하여 모델의 품질을 확인
    ```python
    print(classification_report(y_test,y_pred))
    ```

   정확도가 좋다.

    ```output
                    precision    recall  f1-score   support
    
         chinese       0.72      0.69      0.70       257
          indian       0.91      0.87      0.89       243
        japanese       0.79      0.77      0.78       239
          korean       0.83      0.79      0.81       236
            thai       0.72      0.84      0.78       224
    
        accuracy                           0.79      1199
       macro avg       0.79      0.79      0.79      1199
    weighted avg       0.79      0.79      0.79      1199
    ```

### Onnx에 모델 적용시키기
반드시 적절한 Tensor 수 로 변환해야 한다. 이 데이터 집합에는 380개의 성분이 나열되어 있으므로 'FloatTensorType'에서 해당 숫자를 기록해야 한다.

1. 380개의 Tensor 수 변환

    ```python
    from skl2onnx import convert_sklearn
    from skl2onnx.common.data_types import FloatTensorType
    
    initial_type = [('float_input', FloatTensorType([None, 380]))]
    options = {id(model): {'nocl': True, 'zipmap': False}}
    ```

1. onnx를 생성하고 파일을 'model.onnx'로 저장합니다.

    ```python
    onx = convert_sklearn(model, initial_types=initial_type, options=options)
    with open("./model.onnx", "wb") as f:
        f.write(onx.SerializeToString())
    ```
    
    
## 나의 모델 보기
Onnx 모델은 Visual Studio 코드에서 잘 보이지 않지만, 많은 연구자들이 모델을 시각화하기 위해 사용하는 매우 좋은 무료 소프트웨어가 있다. Netron을 다운로드하고 model.onnx 파일을열면 380개의 입력과 분류기가 나열된 단순 모델을 시각화할 수 있다.

<img width="196" alt="netron" src="https://user-images.githubusercontent.com/103716440/167253252-a61208c8-7e0d-4f07-95ef-e53ece21ac5f.png">

Netron은 나의 모델을 보여주는데 도움을 준다.

## 추천하는 웹 응용 프로그램 구축
이제부터는 내가 지금까지 정리한 데이터세트와 훈련모델을 이용해서 웹 프로그램을 구축하는데 아직 배우지 않은 부분이니 따라하면서 읽어보자.

1. _index.html_에서 다음 마크업을 추가하십시오.

    ```html
    <!DOCTYPE html>
    <html>
        <header>
            <title>Cuisine Matcher</title>
        </header>
        <body>
            ...
        </body>
    </html>
    ```

1. 'body' 태그 내에서 작업하면서 일부 성분을 반영하는 확인란 목록을 표시하기 위해 약간의 마크업을 추가

    ```html
    <h1>Check your refrigerator. What can you create?</h1>
            <div id="wrapper">
                <div class="boxCont">
                    <input type="checkbox" value="4" class="checkbox">
                    <label>apple</label>
                </div>

                <div class="boxCont">
                    <input type="checkbox" value="247" class="checkbox">
                    <label>pear</label>
                </div>

                <div class="boxCont">
                    <input type="checkbox" value="77" class="checkbox">
                    <label>cherry</label>
                </div>

                <div class="boxCont">
                    <input type="checkbox" value="126" class="checkbox">
                    <label>fenugreek</label>
                </div>

                <div class="boxCont">
                    <input type="checkbox" value="302" class="checkbox">
                    <label>sake</label>
                </div>

                <div class="boxCont">
                    <input type="checkbox" value="327" class="checkbox">
                    <label>soy sauce</label>
                </div>

                <div class="boxCont">
                    <input type="checkbox" value="112" class="checkbox">
                    <label>cumin</label>
                </div>
            </div>
            <div style="padding-top:10px">
                <button onClick="startInference()">What kind of cuisine can you make?</button>
            </div> 
    ```

    index.html 파일에서 작업을 계속하고 최종 종료 </div> 뒤에 모델이 호출되는 스크립트 블록을 추가

1. [Onnx Runtime] 불러오기(https://www.onnxruntime.ai/):

    ```html
    <script src="https://cdn.jsdelivr.net/npm/onnxruntime-web@1.9.0/dist/ort.min.js"></script> 
    ```


1. 런타임이 설치되면 런타임을 호출할 수 있다.

    ```html
    <script>
        const ingredients = Array(380).fill(0);
        
        const checks = [...document.querySelectorAll('.checkbox')];
        
        checks.forEach(check => {
            check.addEventListener('change', function() {
                // toggle the state of the ingredient
                // based on the checkbox's value (1 or 0)
                ingredients[check.value] = check.checked ? 1 : 0;
            });
        });
        function testCheckboxes() {
            // validate if at least one checkbox is checked
            return checks.some(check => check.checked);
        }
        async function startInference() {
            let atLeastOneChecked = testCheckboxes()
            if (!atLeastOneChecked) {
                alert('Please select at least one ingredient.');
                return;
            }
            try {
                // create a new session and load the model.
                
                const session = await ort.InferenceSession.create('./model.onnx');
                const input = new ort.Tensor(new Float32Array(ingredients), [1, 380]);
                const feeds = { float_input: input };
                // feed inputs and run
                const results = await session.run(feeds);
                // read from results
                alert('You can enjoy ' + results.label.data[0] + ' cuisine today!')
            } catch (e) {
                console.log(`failed to inference ONNX model`);
                console.error(e);
            }
        }
               
    </script>
    ```
    
이 코드에는 다음과 같은 몇 가지 일이 발생한다.
1. 성분 확인란이 선택되었는지 여부에 따라 380개의 가능한 값(1 또는 0)을 설정하여 모형으로 전송하여 추론
2. 확인란 배열과 응용 프로그램이 시작될 때 호출되는 'init' 함수에서 확인되었는지 확인하는 방법을 만들었다. 확인란을 선택하면 선택한 성분을 반영하도록 성분 배열이 변경됩니다.
3. 확인란이 선택되었는지 확인하는 'test Checkboxes' 함수를 생성.
4. 버튼을 누르면 '추론 시작' 기능을 사용하고, 체크박스가 켜져 있으면 추론을 시작.
5. 추론 루틴은 다음을 포함한다.
   1. 모델의 비동기 로드 설정
   2. 모델에 보낼 Tensor 구조 생성
   3. 모델을 교육할 때 작성한 'float_input' 입력을 반영하는 'feeds' 만들기(Netron을 사용하여 해당 이름을 확인 가능)
   4. 이러한 'feeds'를 모델에 보내고 응답을 기다림

## 어플리케이션 테스트

<img width="816" alt="web-app" src="https://user-images.githubusercontent.com/103716440/167253598-816d6574-f275-4171-9911-0ddddc8ec133.png">

위에 해당하는 스크립트를 적용하면 웹 프로그램을 만들고 결과를 확인할 수 있나보다.

## 소감
**이 github을 통해 그동안 배운 분류기들을 활용해 '음식재료' 라는 데이터셋들을 분류했다. 이번 공부를 통해 데이터셋이 주어졌을때 여러 분류기를 사용해 어떤 것을 사용해야 좋을지 추론하는 능력을 더 기를 수 있었다.**
