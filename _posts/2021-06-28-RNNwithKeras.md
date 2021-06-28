---
layout: post
published: true
title: 케라스를 통해 RNN을 구현해보자!(RNN with Keras)
excerpt:
tags: RNN
author: yunsu
---
# LSTM을 활용한 주식가격 예측 예제  
기본 라이브러리 설정  
```python  
import numpy as np # linear algebra  
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)  
import matplotlib.pyplot as plt  
import seaborn as sns  
```  
## 데이터 불러오기  
```python  
# Mount Google Drive
from google.colab import drive # import drive from google colab

ROOT = "/content/drive"     # default location for the drive
print(ROOT)                 # print content of ROOT (Optional)
drive.mount(ROOT)           # we mount the google drive at /content/drive
```   
```python  
/content/drive
Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount("/content/drive", force_remount=True).
```   
```python  
from os.path import join  

MY_GOOGLE_DRIVE_PATH = 'My Drive/Colab Notebooks/ml_project' # 프로젝트 경로
PROJECT_PATH = join(ROOT, MY_GOOGLE_DRIVE_PATH) # 프로젝트 경로
print(PROJECT_PATH)
```   
```python  
/content/drive/My Drive/Colab Notebooks/ml_project
```   
```python  
%cd "{PROJECT_PATH}"
```   
```python  
/content/drive/My Drive/Colab Notebooks/ml_project
```   
```python  
stocks =  pd.read_csv('data/stock.csv', header=0)
stocks
```   
# 데이터 전처리 및 시각화  
## 날짜형 변환  
```python  
stocks['일자'] = pd.to_datetime(stocks['일자'], format='%Y%m%d')
# stocks['일자'] = pd.to_datetime(stocks['일자'], format='%Y-%m-%d')
stocks['연도'] = stocks['일자'].dt.year
```  
연도를 1990년 이후로 재분류 한다.  
```python  
df = stocks.loc[stocks['일자']>="1990"]
# df = stocks.loc[(stocks['일자']>="1990") & (df['column_name'] <= B)]
```  
거래량과 종가를 기준으로 구분하여 데이터 확인  
```python  
plt.figure(figsize=(16, 9))
sns.lineplot(y=df['거래량'], x=df['일자'])
plt.xlabel('time')
plt.ylabel('price')
```   
```python  
Text(0, 0.5, 'price')
```   
![put image plz](/assets/img/yunsu/rnn.png){: width="1400" height="400"}  

```python  
plt.figure(figsize=(16, 9))
sns.lineplot(y=df['종가'], x=df['일자'])
plt.xlabel('time')
plt.ylabel('price')
```   
```python  
Text(0, 0.5, 'price')
```  
![put image plz](/assets/img/yunsu/rnn2.png){: width="1400" height="400"}  
주식 가격이 2000년대 이후로 계속 우상향하는 것을 확인.  
## 데이터 정규화  
정규화(MinMaxScaler)를 사용하여 전체 데이터를 0~1 사이 값으로 변환  
```python  
from sklearn.preprocessing import MinMaxScaler

df.sort_index(ascending=False).reset_index(drop=True)

scaler = MinMaxScaler()
scale_cols = ['시가', '고가', '저가', '종가', '거래량']
df_scaled = scaler.fit_transform(df[scale_cols])
df_scaled = pd.DataFrame(df_scaled)
df_scaled.columns = scale_cols

df_scaled
```  
## 시계열 데이터의 데이터셋 분리  
```python  
TEST_SIZE = 200
WINDOW_SIZE = 20

train = df_scaled[:-TEST_SIZE]
test = df_scaled[-TEST_SIZE:]
```  

```python  
def make_dataset(data, label, window_size=20):
    feature_list = []
    label_list = []
    for i in range(len(data) - window_size):
        feature_list.append(np.array(data.iloc[i:i+window_size]))
        label_list.append(np.array(label.iloc[i+window_size]))
    return np.array(feature_list), np.array(label_list)
```   
## 데이터셋 분리  
```python  
from sklearn.model_selection import train_test_split

feature_cols = ['시가', '고가', '저가', '거래량']
label_cols = ['종가']

train_feature = train[feature_cols]
train_label = train[label_cols]

train_feature, train_label = make_dataset(train_feature, train_label, 20)

x_train, x_valid, y_train, y_valid = train_test_split(train_feature, train_label, test_size=0.2)
x_train.shape, x_valid.shape
```  
```python  
((6086, 20, 4), (1522, 20, 4))
```   
```python  
test_feature = test[feature_cols]
test_label = test[label_cols]

test_feature.shape, test_label.shape
```  
```python  
((200, 4), (200, 1))
```   
```python  
test_feature, test_label = make_dataset(test_feature, test_label, 20)
test_feature.shape, test_label.shape
```   
```python  
((180, 20, 4), (180, 1))
```  
## 모델 학습  
RNN 아키텍처 생성  
```python  
from keras.models import Sequential
from keras.layers import Dense
from keras.callbacks import EarlyStopping, ModelCheckpoint
from keras.layers import LSTM

model = Sequential()
model.add(LSTM(16, 
               input_shape=(train_feature.shape[1], train_feature.shape[2]), 
               activation='relu', 
               return_sequences=False)
          )

model.add(Dense(1))
```  
Model Fit
```python  
import os

model.compile(loss='mean_squared_error', optimizer='adam')
early_stop = EarlyStopping(monitor='val_loss', patience=5)

model_path = 'model'
filename = os.path.join(model_path, 'tmp_checkpoint.h5')
checkpoint = ModelCheckpoint(filename, monitor='val_loss', verbose=1, save_best_only=True, mode='auto')

history = model.fit(x_train, y_train, 
                                    epochs=200, 
                                    batch_size=16,
                                    validation_data=(x_valid, y_valid), 
                                    callbacks=[early_stop, checkpoint])
```  
```python  
Epoch 1/200
375/381 [============================>.] - ETA: 0s - loss: 0.0089
Epoch 00001: val_loss improved from inf to 0.00013, saving model to model/tmp_checkpoint.h5
381/381 [==============================] - 3s 8ms/step - loss: 0.0088 - val_loss: 1.3481e-04
.
.
.
Epoch 42/200
380/381 [============================>.] - ETA: 0s - loss: 1.7859e-05
Epoch 00042: val_loss did not improve from 0.00002
381/381 [==============================] - 3s 7ms/step - loss: 1.7851e-05 - val_loss: 1.9842e-05
```   
## 주가 예측
predict()를 활용, 모형 예측
```python  
model.load_weights(filename)
pred = model.predict(test_feature)

pred.shape
```   
```python  
(180, 1)
```  
```python  
plt.figure(figsize=(12, 9))
plt.plot(test_label, label = 'actual')
plt.plot(pred, label = 'prediction')
plt.legend()
plt.show()
```  
![put image plz](/assets/img/yunsu/rnn3.png){: width="1400" height="400"}  
# Referenc  
Lee, T. (2020, February 14). 딥러닝(LSTM)을 활용하여 삼성전자 주가 예측을 해보았습니다.  
Retrieved August 27, 2020, from https://teddylee777.github.io/tensorflow/LSTM으로-예측해보는-삼성전자-주가  