---
layout: post
published: true
title: "인공신경망(Artificial Neural Network)과 합성곱 신경망의 구성요소(Component of convolutional neural network)"
Date: 2021-12-06
tags: [study,Artificial Neural Nework,tensorflow,ML,perceptron,Component of convolutional neural network]
author: cindy
---
# 생물학적인 뉴런
- 인간의 뇌는 1000억 개가 넘는 신경세포(뉴런)가 100조 개 이상의 시냅스를 통해 병렬적으로 연결
- 각각의 뉴런은 수상돌기(Dendrite)를 통해 다른 뉴런에서 입력신호를 받아서 축색돌기(Axon)를 통해 다른 뉴런으로 신호를 내보낸다.
- 출력신호는 입력된 신호가 모여서 일정한 용량을 넘어설 때 일어난다.

# 인공신경망 뉴런 [퍼셉트론 (Perceptron)]
- 인공신경망 뉴런 모델은 생물학적인 뉴런을 수학적으로 모델링 하였다. (1개의 뉴런 =Perceptrone)
- 퍼셉트론(perceptron)은 초기 형태 인공신경망의 한 종류로서, 1957년에 코넬 항공 연구소(Cornell Aeronautical Lab)의 프랑크 로젠블라트 (Frank Rosenblatt)에 의해 고안
- 각 노드의 가중치와 입력치를 곱한 것을 모두 합한 값이 활성함수에 의해 판단되는데, 그 값이 임계치(보통 0)보다 크면 뉴런이 활성화되고 결과값으로 1을 출력한다. 
- 뉴런이 활성화되지 않으면 결과값으로 0을 출력 한다.

생물학적 신경세포 구성과 이를 모방한 인공신경망의 원리

![png](/assets/img/Cindy/ann/ANN_2.png)

# 인공신경망 (Artificial Neural Nework, ANN)의 원리
- 인공신경망(ANN)은 생물학의 뇌 구조(신경망)를 모방하여 모델링한 수학적 모델. 즉, 인공신경망은 이러한 생물학적 신경세포의 정보처리 및 전달 과정을 모방하여 구현 한것
- 여러 뉴런이 서로 연결되어 있는 구조의 네트워크
- 각 노드들은 가중치가 있는 링크들로 연결되어 있고, 전체 모델은 가중치를 반복적으로 조정하면서 학습
- 입력층 , 은닉층 ,출력층으로 이루어 진다.

![png](/assets/img/Cindy/ann/ANN_3.png)

##### INPUT LAYER (입력층) - 외부 데이터를 입력하는 입구노드
##### HIDDEN LAYER (은닉층) - 노드 사이를 연결하고 결과를 노드로 전달
##### OUTPUT LAYER (출력층) - 최종 결과를 내보내는 출구 노드

** 활성화 함수(Activation Function) : 입력된 데이터의 가중 합을 출력신호로 변환하는 함수 
ex 시그모이드 함수(sigmoid), ReLU(Rectified Linear Unit),Softmax

- 인공신경망의 한계
  - 1969년 마빈 민스키는 퍼셉트론의 한계를 수학적으로 증명
  - 퍼셉트론은 단순한 선형 분류기에 불과하여 OR 와 AND 와 같은 분류는 할수 있으나, XOR 분류(비선형 분류)를 수행할 수 없다.

![png](/assets/img/Cindy/ann/ANN_4.png)

![png](/assets/img/Cindy/ann/ANN_5.png)
  - 하나의 층을 추가하여 NAND , OR 게이트로 보낸 다음 그 결과값을 AND 게이트로 보내면 XOR 게이트를 구현 가능

==> 심층신경망과 역전파 도입으로 해결

---
# 합성곱 신경망의 구성요소(Component of convolutional neural network)
- 시각적 이미지분석에 가장 일반적으로 사용되는 인공신경망의 한종류로, 입력 이미지로부터 특징을 추출하여 입력이미지가 어떤 이미지인지 클래스를 분류함
- 완전연결(fully-connected)계층과는 달리 합성곱 신경망은 크게 합성곱층과(Convolution layer)와 풀링층(Pooling layer)으로 구성 

![png](/assets/img/Cindy/ann/CNN_1.png)

## 합성곱 계층 (CONV)
- 아래의 그림처럼 포토샵 등에서 다양한 필터를 적용해서 이미지를 변형하는 것처럼, CNN에서도 이미지 인식을 보다 잘 할 수 있도록 필터를 사용
- 필터(filter)는 이미지의 부분 부분을 이동하는데, 한번 움직일 때 이동할 거리를 스트라이드(stride)라고 한다.
- 출력 결과는 피쳐맵(feature map) 또는 활성화맵(activation map)이라고 한다.
![png](/assets/img/Cindy/ann/CNN_0.png)

<img src="https://m.blog.naver.com/msnayana/220776380373?view=img_1">

- 합성곱을 진행할수록 이미지 크기가 줄어든다. -> 이러한 현상을 해결하기 위해 Padding 기법 사용
  
### Padding 처리
- 입력데이터와 출력데이터의 크기를 맞추기 위해 사용

ex) 4x4 이미지가 합성곱을 수행하여 2x2 출력이미지의 크기를 갖는다.
- 이미지의 가장자리에 0의 값을 갖는 픽셀을 추가하는 것을 zero-padding 이라고 한다.
  
![png](/assets/img/Cindy/ann/CNN_3.png)

## 풀링(Pooling)
- 세로, 가로 방향의 공간을 줄이는 연산으로, sub-sampling 이라고도 불림
- 풀링의 종류는 최대풀링, 평균풀링, 최소풀링이 있다.
![png](/assets/img/Cindy/ann/CNN_4.png)

## 완전연결층(Fully-connected layer)의 역할
- 추출된 특징값을 기존 neural network에 넣어서 이미지를 분류함
- 2가지 종류의 Layer가 존재
  - Flatten Layer : 데이터 타입을 fully-connected 네트워크 형태로 변경. 입력데이터의 shape 변경만 수행
  - Softmax Layer : Classification 수행

---
# Souce
ANN
- https://dbrang.tistory.com/1537
- https://bioinformaticsandme.tistory.com/231
- https://edumon.tistory.com/160
- https://brunch.co.kr/@gdhan/6
- https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=stelch&logNo=221611099784
- https://trendy00develope.tistory.com/35
CNN
- https://stanford.edu/~shervine/l/ko/teaching/cs-230/cheatsheet-convolutional-neural-networks
