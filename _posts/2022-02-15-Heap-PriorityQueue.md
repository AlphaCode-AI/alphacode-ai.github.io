---
layout: post
published: true
title: "Heap & Priority Queue"
feature-img: "/assets/img/Gem/Heap/thumbnail.png"
thumbnail:  "/assets/img/Gem/Heap/thumbnail.png"
date: 2022-02-15
excerpt: "Heap & Priority queue"
tags: [Heap list, 자료 구조, 파이썬, 파이썬으로 배우는 자료 구조 핵심 원리, AI, ML, Artificial intelligence, machine learning, megazone, ai center]
author: gem
---

# 힙 <sup>Heap</sup>


## Heap ?

<img src = "https://transcode-v2.app.engoo.com/image/fetch/f_auto,c_lfill,h_128,dpr_2/https://assets.app.engoo.com/images/IJvC4DCoE2cvV5wbsZrb3WebRZGFwkCx5Uw9LOqfmG1.jpeg">


- **'무엇인가를 차곡차곡 쌓아올린 더미'** 를 뜻함
- 완전 이진 트리의 일종
  - 배열로 구현 가능
  - 힙은 배열에서 일반적으로 0번 인덱스를 사용하지 않고 1번 인덱스를 루트로 잡음
- 여러 개의 값들 중에서 최댓값이나 최솟값을 빠르게 찾아내도록 만들어진 자료구조
- 힙은 일종의 반정렬 상태를 유지한다(큰 값이 상위 레벨에 있고 작은 값이 하위 레벨에 있다는 정도)
- 루트 노드에 우선순위가 높은 데이터를 위치시키는 자료구조

- 최대 힙(max heap)과 최소 힙(min heap)이 있음
  - 최대 힙 <sup>Max Heap</sup>
    - 어떤 노드의 키가 자식의 키보다 작지 않은 트리 (직접 연결된 자식-부모 노드 간의 크기만 비교)
  - 최소 힙 <sup>Min Heap</sup>
    - 어떤 노드의 키가 자식의 키보다 크지 않은 트리 (직접 연결된 자식-부모 노드 간의 크기만 비교)
  
- 노드 위 숫자는 배열의 인덱스를 의미함
  - index(parent) = ⎣index(child)/2⎦
  - index(left_child) = index(parent) * 2
  - index(right_child) = index(parent) * 2 + 1

<img src = "https://assets.alexandria.raywenderlich.com/books/dsk/images/622e8fe71e9859f9280d32fb0905df630ee7a735330b21d12c08686ee9a85945/original.png">





##### [최대 힙의 배열 표현]

<img src = "https://upload.wikimedia.org/wikipedia/commons/thumb/3/38/Max-Heap.svg/1200px-Max-Heap.svg.png">


### 최대 힙 삽입(push)
  1) 마지막 원소 다음의 인덱스에 삽입
  2) 부모 노드와 크기 비교 (max/min heap의 특성을 유지하기 위하여)
  3) heap의 특성이 깨져있다면 부모 노드와 바꿈
  4) 부모 노드와 비교하여 heap의 특성이 유지된다면 stop

##### [최대 힙의 push]
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F998E4A345A9AD4532AFF4D">

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile5.uf.tistory.com%2Fimage%2F995493405A9AD49424F9D1">

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile4.uf.tistory.com%2Fimage%2F99D24D395A9AD4D43763AA">


### 최대 힙 삭제(pop)
    1) 힙에서 삭제는 트리의 root 노드만 삭제 및 조회 가능
    2) 루트 노드의 요소를 삭제 ⇒ 완전 이진 트리 x 
    1) 힙의 마지막 요소를 루트 노드로 만듦(temp root) ⇒ 완전 이진 트리 o,최대 힙 특성 x 
    2) 힙의 특성에 도달할 때까지 재배치


<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile5.uf.tistory.com%2Fimage%2F9989F6495A9AD6AB136829">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile30.uf.tistory.com%2Fimage%2F9937B54F5A9AD6E50B5474">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile9.uf.tistory.com%2Fimage%2F99315A425A9AD78F1509DE">
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile28.uf.tistory.com%2Fimage%2F99060B475A9AD7D033F7A7">




```py
#힙에 저장할 요소를 Element class로 정의
class Element:
    def __init__(self, key):
        self.key=key

# MaxHeap 키 속성을 지닌 요소 집합
class MaxHeap:
    MAX_ELEMENTS=100
    def __init__(self):
        # 배열의 1번 인덱스부터 키가 삽입되기 때문에 +1개의 배열 생성
        self.arr=[None for i in range(self.MAX_ELEMENTS+1)]
        #heapsize : 힙에 저장된 키 개수, 힙에있는 마지막 키의 인덱스
        self.heapsize=0

    # 힙이 비어있으면 True, 아니면 False를 반환
    def is_empty(self):
        if self.heapsize==0:
            return True
        return False

    # 힙이 가득 찼으면 True, 아니면 False를 반환
    def is_full(self):
        if self.heapsize>=self.MAX_ELEMENTS:
            return True
        return False

    # 부모 노드의 인덱스를 받아옴
    def parent(self, idx):
        return idx >> 1

    # 왼쪽 자식 노드의 인덱스를 받아옴
    def left(self, idx):
        return idx << 1

    # 오른쪽 자식 노드의 인덱스를 받아옴
    def right(self, idx):
        return (idx << 1) + 1


    # 힙에 요소를 삽입
    def push(self, item):
        if self.is_full():
            raise IndexError("the heap is full!!")

        # 마지막 원소의 다음 인덱스
        self.heapsize+=1
        cur_idx=self.heapsize

        #cur_idx가 루트가 아니고
        #item의 key가 cur_idx 부모의 키보다 크면
        while cur_idx!=1 and item.key > self.arr[self.parent(cur_idx)].key:
            self.arr[cur_idx]=self.arr[self.parent(cur_idx)]
            cur_idx=self.parent(cur_idx)
        self.arr[cur_idx]=item

    # 힙에서 최대 원소를 삭제하면서 반환
    def pop(self):
        if self.is_empty():
            return None

        #삭제된 후 반환될 원소
        rem_elem=self.arr[1]

        # 맨 마지막에 위치한 원소를 받아온 후
        # 힙 사이즈를 줄이면 완전 이진 트리 특성을 유지할 수 있다.
        temp=self.arr[self.heapsize]
        self.heapsize-=1

        #루트에서 시작
        cur_idx=1
        # 루트의 왼쪽 자식
        child = self.left(cur_idx)
    
        # 만약 child > heapsize 이면 
        # arr[cur_idx]는 리프 노드이다
        while child <= self.heapsize:
            # 오른쪽 자식이 있고
            # 오른쪽 자식의 키가 왼쪽 자식의 키보다 크면
            # child를 오른쪽 자식으로
            if child < self.heapsize and \
               self.arr[self.left(cur_idx)].key < self.arr[self.right(cur_idx)].key:
                child = self.right(cur_idx)
            # 최대 힙 특성을 만족하면 
            # 반복문을 나온다.
            if temp.key >= self.arr[child].key:
                break
            # 키가 큰 자식 원소를 부모로 이동시킨다
            # cur_idx는 자식 원소로 이동한다.
            self.arr[cur_idx]=self.arr[child]
            cur_idx=child
            child=self.left(cur_idx)
        
        self.arr[cur_idx]=temp

        return rem_elem


# 힙 내부에 있는 arr를 
# 직접 확인하기 위한 함수
def print_heap(h):
    for i in range(1, h.heapsize+1):
        print("{}".format(h.arr[i].key), end="  ")
    print()

if __name__=="__main__":
    h=MaxHeap()

    h.push(Element(2))
    h.push(Element(14))
    h.push(Element(9))
    h.push(Element(11))
    h.push(Element(6))
    h.push(Element(8))

    print_heap(h)

    while not h.is_empty():
        rem=h.pop()
        print(f"poped item is {rem.key}")
        print_heap(h)
```


# 우선순위 큐 <sup>Priority Queue</sup>

## Priority Queue?

<img src = "https://media.vlpt.us/post-images/holicme7/b77edff0-1ff7-11ea-b8b0-5340b200eb0f/image.png">

- 최대 우선순위 큐(Max Priority Queue)와 최소 우선순위 큐(Min Priority Queue)가 있음
- 우선순위 큐는 힙으로 구현하는 경우가 많음


```py
from heapq import heappush, heappop

class Element:
    def __init__(self, key, string):
        self.key=key
        self.data=string

class MinPriorityQueue:
    def __init__(self):
        self.heap=[]

    def is_empty(self):
        if not self.heap:
            return True
        return False

    def push(self, item):
        heappush(self.heap, (item.key, item.data))

    def pop(self):
        return heappop(self.heap)

    def min(self):
        return self.heap[0]

if __name__=="__main__":
    pq=MinPriorityQueue()
    
    pq.push(Element(2, "kim"))
    pq.push(Element(14, "park"))
    pq.push(Element(9, "choi"))
    pq.push(Element(11, "lee"))
    pq.push(Element(6, "yang"))
    pq.push(Element(8, "jang"))

    while not pq.is_empty():
        elem=pq.pop()
        print(f"key[{elem[0]}] : data[{elem[1]}]")


```