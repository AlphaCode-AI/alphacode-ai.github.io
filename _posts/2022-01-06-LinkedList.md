---
layout: post
published: true
title: "Linked list"
date: 2022-01-06
excerpt: "Linked list"
tags: [Linked list, 자료 구조, 파이썬, 파이썬으로 배우는 자료 구조 핵심 원리, AI, ML, Artificial intelligence, machine learning, megazone, ai center]
author: gem
---

# 연결리스트(Linked List)


## 노드(node)
- 데이터들을 갖고 있는 요소들이 참조로 이어져 있음
- 노드는 데이터를 담는 부분과 다음 노드를 가리키는 참조로 구성
- 이렇게 데이터와 데이터를 연결(link) 해두었기 때문에 **연결리스트(Linked List)** 라고 함

## 연결리스트(Linked List)
- 포인터를 이용하여 데이터를 저장하는 자료 구조 
- 물리적 구조가 순차적이지 않음
- 포인터를 이용하여 데이터를 저장하므로 데이터가 메모리에 인접해 있지는 않음
- 삽입과 삭제가 빈번한 경우 사용


![png](/assets/img/Gem/LinkedList/1.png)

## 연결리스트(Linked List)의 종류
- 
1. 단순 연결 리스트(Single Linked List)
   - 다음 노드를 가리키는 참조 하나만 가지고 있음
   
2. 이중 연결 리스트(Double Linked List)
   - 앞 노드를 가리키는 참조와 뒤 노드를 가리키는 참조를 모두 가지고 있음 



![png](/assets/img/Gem/LinkedList/2.png)


## 연결리스트의 삽입, 삭제, 탐색

1. 삽입
   > 연결리스트
   > - 새로운 노드 생성
   > - 앞 노드의 링크를 생성된 노드로 하고, 생성된 노드의 링크를 뒷 노드로 한다
   
   > 배열
   > - 배열의 마지막에 데이터를 추가하거나 삭제하는 경우를 제외하고 O(n)의 성능을 가짐
   > - 느린 삽입/삭제

   ![png](/assets/img/Gem/LinkedList/3.png)

2. 삭제
    > 연결리스트
    > - 삭제할 노드 앞 노드의 링크를 뒷 노드로 연결시킨다.
    
    > 배열
    > - 배열의 마지막에 데이터를 추가하거나 삭제하는 경우를 제외하고 O(n)의 성능을 가짐
    > - 느린 삽입/삭제

    ![png](/assets/img/Gem/LinkedList/4.png)

    
3. 탐색
    > 연결리스트
    > - 리스트 처음부터 끝까지 하나씩 방문하면서 해당 요소를 비교하여 찾음
    
    > 배열
    > - 단 한번의 연산으로 해당 인덱스의 데이터에 접근 가능



## 더미 이중 연결 리스트(dummy double linked list)
- 보통 연결리스트라 하면 dummy double linked list를 의미
- 더미 노드를 사용
  - 더미노드를 사용할 경우 head 혹은 tail 의 더미노드를 기준으로 " 그 뒤(앞)로 몇 번 가서 삽입, 그 뒤(앞)로 몇 번 가서 삭제"라는 하나의 로직으로 동작이 가능하기 때문에 일반화 시킬 수 있음
- head, tail이 가리키는 더미노드에는 데이터가 저장되어있지 않음
  
   ![png](/assets/img/Gem/LinkedList/5.png)

노드 구현
- data 와 참조할 prev / next 생성


```py
class Node:
    def __init__(self, data=None):
        self.__data=data
        self.__prev=None
        self.__next=None


    # 소멸자 : 객체가 사라지기 전 반드시 호출됩니다.
    # 삭제 연산 때 삭제되는 것을 확인하기 위해
    # 작성하였습니다.
    def __del__(self):
        print("data of {} is deleted".format(self.data))

    @property
    def data(self):
        return self.__data
    
    @data.setter
    def data(self, data):
        self.__data=data
    
    @property
    def prev(self):
        return self.__prev

    @prev.setter
    def prev(self, p):
        self.__prev=p

    @property
    def next(self):
        return self.__next

    @next.setter
    def next(self, n):
        self.__next=n
```


![png](/assets/img/Gem/LinkedList/6.png)


```py
class DoubleLinkedList:
    def __init__(self):
        #더미노드
        self.head=Node()
        self.tail=Node()
        
        #초기화
        #head, tail 연결
        self.head.next=self.tail
        self.tail.prev=self.head
        
        self.d_size=0

    def empty(self):
        if self.d_size==0:
            return True
        else:
            return False

    def size(self):
        return self.d_size

    def add_first(self, data):
        #새로운 노드 만듦
        new_node=Node(data)
        
        #새로운 노드를 더미노드 다음을 가리키도록 함
        new_node.next=self.head.next
        #prev는 리스트의 맨 앞 더미를 가리키도록 함
        new_node.prev=self.head

        #첫번째 데이터노드의 prev가 새로운 노드를 가리키도록
        self.head.next.prev=new_node
        #더미노드의 next는 새로운 노드 가리키도록(새로운 노드 삽입)
        self.head.next=new_node

        # d_size 증가시킴
        self.d_size+=1

    def add_last(self, data):
        new_node=Node(data)

        new_node.prev=self.tail.prev
        new_node.next=self.tail

        self.tail.prev.next=new_node
        self.tail.prev=new_node

        self.d_size+=1

    def insert_after(self, data, node):
        new_node=Node(data)

        new_node.next=node.next
        new_node.prev=node

        node.next.prev=new_node
        node.next=new_node

        self.d_size+=1

    def insert_before(self, data, node):
        new_node=Node(data)

        new_node.prev=node.prev
        new_node.next=node

        node.prev.next=new_node
        node.prev=new_node

        self.d_size+=1

    def search_forward(self, target):
        cur=self.head.next
        
        while cur is not self.tail:
            if cur.data==target:
                return cur
            cur=cur.next
        return None

    def search_backward(self, target):
        cur=self.tail.prev
        while cur is not self.head:
            if cur.data==target:
                return cur
            cur=cur.prev
        return None
        
    def delete_first(self):
        if self.empty():
            return
        self.head.next=self.head.next.next
        self.head.next.prev=self.head

        self.d_size-=1

    def delete_last(self):
        if self.empty():
            return
        self.tail.prev=self.tail.prev.prev
        self.tail.prev.next=self.tail

        self.d_size-=1

    def delete_node(self, node):
        node.prev.next=node.next
        node.next.prev=node.prev

        self.d_size-=1
```