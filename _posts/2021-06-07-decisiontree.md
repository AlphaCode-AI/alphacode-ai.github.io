---
layout: post
title: 5-1. 결정 트리
date: 2021-06-07
author: naran
tags: [Machine learning, Decision Tree, Algorithm, Classification, Regression, Study]
---

### Decision Tree


![](https://annalyzin.files.wordpress.com/2016/07/decision-trees-titanic-tutorial.png) 

<sup>이미지출처 : ALGOBEANS https://algobeans.com/2016/07/27/decision-trees-tutorial/</sup>


- 결과 모델이 Tree 구조를 가지고 있기 때문에 Decision Tree
- 스무고개 하듯이 예/아니오 질문을 이어 나가면서 학습
- 분류와 회귀 문제에 널리 사용하는 모델
	- 분류 : 리프 노드에 가장 많은 클래스가 예측 클래스가 됨
	- 회귀 : 리프 노드에 도달한 샘플의 타겟을 평균하여 예측값으로 사용
- 앙상블 학습 알고리즘의 기반
	- 앙상블 학습은 신경망과 함께 가장 높은 성능을 내기 때문에 인기가 높음

---

### 불순도 <sup>Impurity</sup>

![](/assets/img/feature-img/dt.png)

- 분기 기준을 선택하기 위해 불순도라는 개념을 사용
- 결정 트리가 최적의 질문을 찾기 위한 기준
- 복잡성을 의미하며, 해당 범주 안에 서로 다른 데이터가 얼마나 섞여 있는지를 뜻함
- 다양한 개체들이 섞여 있을수록 불순도가 높아짐
- 분기 기준 설정 시 현재노드의 불순도에 비해 자식노드의 불순도가 감소되도록 설정해야함


---

### 정보 획득 <sup>Information gain</sup>
- 현재 노드의 불순도와 자식노드의 불순도의 차이를 정보 획득<sup>Information gain</sup>이라고 함
- 불순도가 1인 상태에서 0.7인 상태로 바뀌었다면 정보 획득은 0.3
- 결정 트리 알고리즘은 정보 획득을 최대화하는 방향으로 학습이 진행됨
	1. root 노드의 불순도 계산
	2. 나머지 속성에 대해 분할 후 자식노드의 불순도 계산
	3. 각 속성에 대한 정보 획득 계산 후, 정보  획득이 최대가 되는 분기조건을 찾아 분기
	4. 모든 Leaf노드의 불순도가 0이 될 때까지 2,3 반복 수행

---

### 불순도 함수 Gini, Entropy
- 지니 지수 <sup>Gini Index</sup>
	- 클래스의 비율을 제곱해서 더한 다음 1에서 뺌
	- 지니 지수의 0.5면 불순도가 최대 => 한 범주 안에 서로 다른 데이터가 정확히 반반 있다
	- 공식
	> $$ 
	\begin{split}
	I(A) &= 1 - (음성 클래스 비율^2 + 양성 클래스 비율^2) \\
	&= 1 - \textstyle\sum_{k=1}^m p_k^2 
	\end{split}
	$$ 
	- 
	> $$ p_k^2 = 범주 안에서 k 특성의 관측치 비율$$


	- ![Decision Tree](/assets/img/feature-img/dt_impurity.png)

	- 범주 안에 빨간색 점 10개, 파란색 점 6개 있을 때의 계산 예제
	> $$ 
	\begin{split} 
	I(A) &= 1 - \textstyle\sum_{k=1}^m p_k^2 \\
	&= 1 - \left(\cfrac{6}{16}\right)^2 - \left(\cfrac{10}{16}\right)^2 \\
	&\approx 0.47
	\end{split}
	$$


- 엔트로피 지수 <sup>Entropy Index</sup>
	- 불순도를 수치적으로 나타낸 척도
	- 엔트로피가 높다는 것은 불순도가 높다는 뜻이고, 엔트로피가 낮다는 것은 불순도가 낮다는 뜻
	- 엔트로피가 1이면 불순도가 최대 => 한 범주 안에 서로 다른 데이터가 정확히 반반 있다
	- 엔트로피가 0이면 불순도가 최소 => 한 범주 안에 하나의 데이터만 있다
	- 클래스의 비율을 밑이 2인 로그를 사용하여 곱함
	- 공식
	> $$
	\begin{split}
	E(A) &= - 음성 비율 * \log_2(음성 비율) - 양성 비율 * \log_2(양성 비율) \\
	&= - \textstyle\sum_{i=1}^k p_i\log_2(p_i) 
	\end{split}
	$$

	- ![Decision Tree](/assets/img/feature-img/dt_impurity.png)

	- 범주 안에 빨간색 점 10개, 파란색 점 6개 있을 때의 계산 예제
	> $$ 
	\begin{split} 
	E(A) &= - \textstyle\sum_{k=1}^m p_k^2 \\
	&= - \cfrac{6}{16}\log_2\left(\cfrac{6}{16}\right) - \cfrac{10}{16}\log_2\left(\cfrac{10}{16}\right) \\
	&\approx 0.95
	\end{split}
	$$


---

### 가지치기 <sup>Pruning</sup>
- 결정 트리는 제한 없이 성장하면 훈련 세트에 과대적합<sup>Overfitting</sup>되기 쉬움
- 과대적합<sup>Overfitting</sup> 을 막기 위한 전략
- 트리의 최대 깊이(max_depth)나 터미널 노드의 최대 개수, 한 노드가 분할하기 위한 최소 데이터 수(min_sample_split)등 제한 가능


---

### 장점
- 분석결과가 '조건A && 조건B = 결과 C'라는 형태의 규칙으로 표현되므로 쉽게 이해하고 활용 할 수 있음
- 특성값의 스케일이 알고리즘에 영향을 미치지 않아 표준화 전처리를 할 필요가 없음
- 특성 중요도<sup>특성이 불순도를 감소하는데 기여한 정도</sup>를 계산 할 수 있음
	- 특성 선택에 확용할 수 있음

---

