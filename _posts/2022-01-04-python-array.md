---
Layout: post
Published: true
title: "[Data structure] 03. 배열"
date: 2022-01-04
excerpt: "변수가 한 곳에 모여있으면 빠르다!"
tags: [python, data structure, megazone, ai center]
author: debbie
---




# 1. 동적 배열

- 모든 언어에서 사용되는 가장 기본적인 자료구조
- 여러 데이터를 한 곳에 저장할 때 가장 먼저 고려



## Memory Structure

![png](/assets/img/yunju/array/memory.png)

- os는 프로그램의 실행을 위해 다양한 메모리 공간 제공 중

- Stack 
  - 함수의 호출과 관계되는 지역 변수와 매개변수 저장되는 영역
  - 스택 영역은 함수의 호출과 함께 할당되며, 함수의 호출이 완료되면 소멸
  - 매우 빠른 액세스와 변수의 명시적 할당해제 불필요
  - 변수의 크기 조정 불가
  
- Heap
  - 사용자가 직접 관리하는 메모리 영역
  - 사용자에 의해 메모리 공간이 동적으로 할당되고 해제
  - 메모리 크기 제한 없음
  - 메모리를 관리해야함 (변수를 직접 할당하고 해제해야 함)

- 힙 영역에 저장되는 배열 -> Dynamic array
  - Python: List
  - C++: vector
  - Java: ArrayList
  
  

## Abstract Data Type



### 1) is_empty()

- 파이썬에서 빈 리스트는 거짓을 의미

```python
test = []
print(bool(test))
```

```
False
```



### 2) append(element)

- append() 메서드가 리스트의 마지막에 원소를 추가하는 연산 수행

```python
test = [1,2,3,4,5,6]
test.append(7)
print(test)
```

```
[1, 2, 3, 4, 5, 6, 7]
```



### 3) insert(index, element)

- insert() 메서드가 리스트의 중간에 원소를 추가하는 연산

```python
test = [1,2,3,5]
test.insert(1,4)
print(test)
```

```
[1, 4, 2, 3, 5]
```



### 4) remove_last()

- pop() 메서드에 파라미터 전달하지 않으면 리스트의 마지막 원소 삭제

```python
test = [1,4,2,3,5]
test.pop()
print(test)
```

```
[1, 4, 2, 3]
```



### 5) remove(index)

- pop 메서드에 파라미터로 인덱스 전달 시 리스트의 인덱스 위치에 있는 원소 삭제

```python
test = [1,4,2,3]
test.pop(2)
print(test)
```

```
[1, 4, 3]
```



# 2. 지역성의 원리와 캐시



## 지역성의 원리 (principle of locality)

- 프로세스 내 명령어 및 데이터에 대한 참조가 군집화 경향이 있음을 뜻함
- 공간 지역성 (spatial locality)
  - 이번에 접근한 변수는 이전에 접근한 변수 근처에 있을 가능성이 높음

⭐️ 배열은 메모리 상에서 물리적, 선형적으로 이어져 있다. ⭐️

![png](/assets/img/yunju/array/locality.png)

## 캐시 (cache)

- 데이터나 값을 미리 복사해 놓는 임시 장소
- 배열 중의 한 element를 메모리에게 요청할 경우 배열이 통으로 캐시에 올라간다.
- 지역성의 원리로 배열의 한 element가 요청 될 경우 다른 element들도 요청될 확률이 높다.
  - 이 때 다른 element가 요청될 경우 캐시에서 가져오면 됨
  - 메모리에서 데이터 가져 올 때 20 ~100 클럭 소요
  - 캐시에서 데이터 가져올 때 3클럭 소요
  - 성능은 월등하게 향상



# 3. 인덱싱 (Indexing)

- 인덱스: 리스트에서 element의 위치
- 인덱싱: 특정 위치의 element를 가져오는 것
- 데이터 주소 값 = 배열의 첫 주소 값 + (데이터 크기 * 인덱스)
  - 한 번의 연산으로도 값에 접근 가능 -> O(1)
  - 데이터의 개수 상관 없음

![png](/assets/img/yunju/array/indexing.png)

## 빅오 표기법

```python
n = 10
result = 0
for i in range(1, n+1):
  for j in range(i):
    result += 1
print(result)
```

-> n(n+1)/2 번 계산 : O(n^2)



```python
n = 10
result = 0
for i in range(1, n+1):
  result += i
print(result)
```

-> n번 계산 : O(n)



```python
n = 10
result = n*(n+1)/2
print(result)
```

-> 1번 계산 : O(1)



- 위 코드 들은 n의 값이 작다면 큰 차이 없게 느껴지지만 n의 값이 커진다면 효율성의 차이가 커진다

![png](/assets/img/yunju/array/bigo.png)





# 4. 데이터 삽입, 삭제

- capacity : 배열이 확보한 메모리 공간
- size: 채워진 데이터 크기



## 1) 배열 맨 마지막에 추가

- Capacity가 size보다 큰 경우
  - 단순히 마지막 요소 다음에 새로운 요소 추가하면 됨 : O(1)
- 배열이 가득 차있는 경우
  - 충분한 공간 다시 확보 후 기존 배열요소를 모두 복사한 후 새로운 데이터를 추가해야 함.
  - 데이터 개수만큼 복사해야 하므로 O(n)
  - capacity가 충분히 커서 추가하는 동안은 O(1)이지만 배열이 꽉 찼을 때 한번 O(n)
  - 동적 배열에서 배열이 가득 찼을 때 배열 크기의 2배만큼 capacity 잡음



## 2) 배열 맨 마지막 삭제

- 배열의 맨 마지막 요소만 삭제
- 메모리를 0으로 초기화 할 필요 없고 요소만 삭제해도 메모리 해제
- O(1)



## 3) 배열 중간에 데이터 추가

- 이미 있는 요소를 모두 한 번 씩 뒤로 옮김
- 그 후 빈 자리에 새로운 요소 삽입
- 데이터 개수가 n이라면 최악의 경우 맨앞에 추가해야 하므로 O(n)

![png](/assets/img/yunju/array/insert.png)

![png](/assets/img/yunju/array/insert_result.png)

 ## 4) 배열 중간의 데이터 삭제

- 맨 앞의 데이터 (4)를 삭제하려면 뒤에 있는 데이터 한 칸씩 앞으로 옮겨야 함
- O(n)

