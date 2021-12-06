---
layout: post
published: true
title: "3-3. 특성공학과 규제 (실습)"
date: 2021-11-24
excerpt: "특성공학과 규제 실습"
tags: [Feature Engineering, Regularization, AI, ML, Artificial intelligence, machine learning, megazone, ai center]
author: yunju
---


# Feature Engineering

- 모델의 정확도를 높이기 위해 주어진 feature를 조합하여 새로운 feature를 뽑아내는 작업




## 데이터 준비

- 농어의 길이, 높이, 두께 데이터가 feature
- 농어의 무게 데이터가 target


```python
import pandas as pd
```


```python
df = pd.read_csv("https://bit.ly/perch_csv_data")
perch_full = df.to_numpy()
```


```python
perch_full
```

    array([[ 8.4 ,  2.11,  1.41],
           [13.7 ,  3.53,  2.  ],
           [15.  ,  3.82,  2.43],
           [16.2 ,  4.59,  2.63],
           [17.4 ,  4.59,  2.94],
           [18.  ,  5.22,  3.32],
           [18.7 ,  5.2 ,  3.12],
           [19.  ,  5.64,  3.05],
           [19.6 ,  5.14,  3.04],
           [20.  ,  5.08,  2.77],
           [21.  ,  5.69,  3.56],
           [21.  ,  5.92,  3.31],
           [21.  ,  5.69,  3.67],
           [21.3 ,  6.38,  3.53],
           [22.  ,  6.11,  3.41],
           [22.  ,  5.64,  3.52],
           [22.  ,  6.11,  3.52],
           [22.  ,  5.88,  3.52],
           [22.  ,  5.52,  4.  ],
           [22.5 ,  5.86,  3.62],
           [22.5 ,  6.79,  3.62],
           [22.7 ,  5.95,  3.63],
           [23.  ,  5.22,  3.63],
           [23.5 ,  6.28,  3.72],
           [24.  ,  7.29,  3.72],
           [24.  ,  6.38,  3.82],
           [24.6 ,  6.73,  4.17],
           [25.  ,  6.44,  3.68],
           [25.6 ,  6.56,  4.24],
           [26.5 ,  7.17,  4.14],
           [27.3 ,  8.32,  5.14],
           [27.5 ,  7.17,  4.34],
           [27.5 ,  7.05,  4.34],
           [27.5 ,  7.28,  4.57],
           [28.  ,  7.82,  4.2 ],
           [28.7 ,  7.59,  4.64],
           [30.  ,  7.62,  4.77],
           [32.8 , 10.03,  6.02],
           [34.5 , 10.26,  6.39],
           [35.  , 11.49,  7.8 ],
           [36.5 , 10.88,  6.86],
           [36.  , 10.61,  6.74],
           [37.  , 10.84,  6.26],
           [37.  , 10.57,  6.37],
           [39.  , 11.14,  7.49],
           [39.  , 11.14,  6.  ],
           [39.  , 12.43,  7.35],
           [40.  , 11.93,  7.11],
           [40.  , 11.73,  7.22],
           [40.  , 12.38,  7.46],
           [40.  , 11.14,  6.63],
           [42.  , 12.8 ,  6.87],
           [43.  , 11.93,  7.28],
           [43.  , 12.51,  7.42],
           [43.5 , 12.6 ,  8.14],
           [44.  , 12.49,  7.6 ]])


```python
import numpy as np
perch_weight = np.array([5.9, 32.0, 40.0, 51.5, 70.0, 100.0, 78.0, 80.0, 85.0, 85.0, 110.0,
       115.0, 125.0, 130.0, 120.0, 120.0, 130.0, 135.0, 110.0, 130.0,
       150.0, 145.0, 150.0, 170.0, 225.0, 145.0, 188.0, 180.0, 197.0,
       218.0, 300.0, 260.0, 265.0, 250.0, 250.0, 300.0, 320.0, 514.0,
       556.0, 840.0, 685.0, 700.0, 700.0, 690.0, 900.0, 650.0, 820.0,
       850.0, 900.0, 1015.0, 820.0, 1100.0, 1000.0, 1100.0, 1000.0,
       1000.0])
```



테스트 세트와 트레이닝 세트로 나눔


```python
from sklearn.model_selection import train_test_split
train_input, test_input, train_target, test_target = train_test_split(
    perch_full, perch_weight, random_state=42
)
```

```python
lr = LinearRegression()
lr.fit(train_input, train_target)
```

    LinearRegression()

```python
lr.score(train_input, train_target)
```

    0.9559326821885706

```python
lr.score(test_input, test_target)
```

    0.8796419177546367



## 새로운 feature 생성

- 사이킷런에서 제공하는 변환기 사용하여 새로운 feature 생성 가능

```python
from sklearn.preprocessing import PolynomialFeatures
```


```python
poly = PolynomialFeatures(include_bias=False)
poly.fit(train_input)
train_poly = poly.transform(train_input)
```

feature 6개 추가생성

```python
train_poly.shape
```

    (42, 9)



- get_feature_names()
- 각 feature가 어떤 입력의 조합으로 만들어졌는지 알 수 있음


```python
poly.get_feature_names()
```

    /Users/mz01-lyoonj/opt/miniconda3/envs/ml/lib/python3.7/site-packages/sklearn/utils/deprecation.py:87: FutureWarning: Function get_feature_names is deprecated; get_feature_names is deprecated in 1.0 and will be removed in 1.2. Please use get_feature_names_out instead.
      warnings.warn(msg, category=FutureWarning)
    
    ['x0', 'x1', 'x2', 'x0^2', 'x0 x1', 'x0 x2', 'x1^2', 'x1 x2', 'x2^2']



## 다중회귀모델 훈련


```python
from sklearn.linear_model import LinearRegression
```


```python
lr = LinearRegression()
lr.fit(train_poly, train_target)
```
    LinearRegression()



- 매우 높은 점수 나옴
- feature가 늘어나면 선형회귀 능력은 매우 강해짐


```python
lr.score(train_poly, train_target)
```

    0.9903183436982124

```python
test_poly = poly.transform(test_input)
print(lr.score(test_poly, test_target))
```

    0.9714559911594134




 - 특성을 더 많이 추가해보자
 - degree 매개변수 활용해서 고차항의 최대차수 지정 가능


```python
poly = PolynomialFeatures(degree=5, include_bias=False)
poly.fit(train_input)
train_poly = poly.transform(train_input)
test_poly = poly.transform(test_input)
print(trans_poly.shape)
```

    (42, 55)

```python
lr2 = LinearRegression()
lr2.fit(train_poly, train_target)
print(lr2.score(train_poly, train_target))
```

    0.9999999999991097




- 테스트 세트 점수가 음수로 나옴
- 선형모델이 지나치게 강력해져서 훈련세트에 overfitting된 상황
- overfitting을 줄이기 위해서 feature를 줄여야 함


```python
lr2.score(test_poly, test_target)
```

    -144.40579242684848



# Regularization 

- 참고 : https://light-tree.tistory.com/125

- 머신러닝 모델이 너무 과하게 학습하지 못하도록 훼방하는 것
- 즉 모델이 train set에 overfitting되지 않게 하는 것
- 선형 회귀 모델에 regularization을 추가한 모델 : Ridge, Lasso


```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
scaler.fit(train_poly)
train_scaled = scaler.transform(train_poly)
test_scaled = scaler.transform(test_poly)
```




## Norm
![png](/assets/img/yunju/fe_code/norm.png)

- norm : 벡터의 크기(or 길이)를 측정하는 방법(or 함수)
- L2 norm : 초록색
- L1 norm : 초록색 외의 모든 거리



### L2 loss
![png](/assets/img/yunju/fe_code/l2loss.png)

- L2 norm의 제곱의 합



### L1 loss
![png](/assets/img/yunju/fe_code/l1loss.png)

- 실제값과 예측치 사이의 차의 절대값들의 합



### L1 Regularization
![png](/assets/img/yunju/fe_code/l1r.png)

- cost function에 가중치의 절대값을 더해준다
- 가중치가 크지 않은 방향으로 학습된다.
- 학습률이 0일 경우 정규화 효과 없음
- Lasso 가 이에 해당



### L2 Regularization
![png](/assets/img/yunju/fe_code/l2r.png)

- cost function에 가중치의 제곱을 더해준다.
- L1 Regularization과 마찬가지로 가중치가 크지 않은 방향으로 학습된다.
- Ridge 가 이에 해당



## Ridge

- 계수를 제곱한 값을 기준으로 규제 적용


```python
from sklearn.linear_model import Ridge

ridge=Ridge()
ridge.fit(train_scaled, train_target)
print(ridge.score(train_scaled, train_target))
```

    0.9896101671037343

```python
ridge.score(test_scaled, test_target)
```
    0.9790693977615391



- 규제의 양을 임의로 조절 가능
- alpha 매개변수로 규제 강도 조절
- alpha 값이 크면 규제 강도가 세짐 -> 계수 값을 더 줄이고 과소적합되도록 유도

- 적절한 alpha 값 찾기 : alpha 값에 대한 R^2 값의 그래프 그려보기
- train set과 test set의 점수가 가장 가까운 지점이 최적의 alpha 값


```python
import matplotlib.pyplot as plt

train_score = list()
test_score = list()

alpha_list = [0.001, 0.01, 0.1, 1, 10, 100]

for alpha in alpha_list:
    ridge = Ridge(alpha = alpha)
    ridge.fit(train_scaled, train_target)
    
    train_score.append(ridge.score(train_scaled, train_target))
    test_score.append(ridge.score(test_scaled, test_target))
```


```python
plt.plot(np.log10(alpha_list), train_score)
plt.plot(np.log10(alpha_list), test_score)

plt.xlabel('alpha')
plt.ylabel('score')
plt.show()
```


​    
![png](/assets/img/yunju/fe_code/output_42_0.png)
​    


적절한 alpha 값 : 0.1


```python
ridge = Ridge(alpha=0.1)
ridge.fit(train_scaled, train_target)
print(ridge.score(train_scaled, train_target))
print(ridge.score(test_scaled, test_target))
```

    0.9903815817570365
    0.9827976465386884




## Lasso

계수의 절댓값을 기준으로 규제 적용


```python
from sklearn.linear_model import Lasso
lasso = Lasso()
lasso.fit(train_scaled, train_target)
print(lasso.score(train_scaled, train_target))
```

    0.989789897208096

```python
lasso.score(test_scaled, test_target)
```
    0.9800593698421883



```python
train_score = list()
test_score = list()

alpha_list = [0.001, 0.01, 0.1, 1, 10, 100]

for alpha in alpha_list:
    lasso = Lasso(alpha = alpha)
    lasso.fit(train_scaled, train_target)
    
    train_score.append(lasso.score(train_scaled, train_target))
    test_score.append(lasso.score(test_scaled, test_target))
```


```python
plt.plot(np.log10(alpha_list), train_score)
plt.plot(np.log10(alpha_list), test_score)

plt.xlabel('alpha')
plt.ylabel('score')
plt.show()
```


​    
![png](/assets/img/yunju/fe_code/output_52_0.png)
​    



```python
lasso = Lasso(alpha=10)
lasso.fit(train_scaled, train_target)
print(lasso.score(train_scaled, train_target))
print(lasso.score(test_scaled, test_target))
```

    0.9888067471131867
    0.9824470598706695

