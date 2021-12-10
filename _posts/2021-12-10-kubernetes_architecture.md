---
layout: post
published: true
title: "[Kubernetes] Kubernetes Architecture"
Date: 2021-12-10
tags: [study,docker,Kubernetes,namespace]
author: debbie
---

# 1. Kubernetes 동작원리

- 쿠버네티스의 컨테이너가 어떻게 만들어져서 운영까지의 흐름이 어떤 형태로 동작하는지

 ![png](/assets/img/yunju/k8s_architecture/container_flow.png)

- 개발자 (or 운영자)가 컨테이너를 k8s 플랫폼에 올려서 실행하려는것이 목적

1) 컨테이너 빌드
2) docker 명령으로 hub에 push
3) kubectl 명령으로 k8s에게 이 컨테이너가 실행되도록 명령 (yml, cli ...)
4) 만들어진 kubectl 명령어 control-plane에게 보냄
5) Control-plane에 있는 rest api server가 받아줌
6) 스케줄러가 어떤 작업 노드에 배치하면 좋을지 결정. 결정된 노드의 kubelet에 실행 요청
7) 요청 받은 노드는 kubelet 명령어를 docker로 변경하여 docker daemon에게 실제 컨테이너 실행 요청
8) docker daemon은 hub에서 해당 컨테이너 이미지 search한 후에 pull , run
9) k8s는 이렇게 동작되는 컨테이너를 pod라는 단위로 관리



## kubernetes component

1. Control plane component

![png](/assets/img/yunju/k8s_architecture/master_component.png)

- Kube-apiserver

  - kubectl 명령으로 된 요청 받는 컴포넌트
  - 문법, 권한 등이 합당한지 검사 -> 합당하면 실행

- etcd storage

  - Key-value 타입으로 저장
  - worker node들에 대한 상태 정보 (hw 리소스, 동작중인 컨테이너 상태...)
  - kubelet 안에는 cAdvisor 라는 컨테이너 모니터링 툴이 포함되어 있음
  - cAdvisor가 수집한 정보들을 control-plane안에 있는 etcd에 저장
  - kubelet은 daemon이라서 계속 진행됨

- scheduler

  - api로 받은 요청을 문법 체크한 후 etcd에 있는 worker node 상태정보 받아서 scheduler에게 요청
  - 어떤 worker node에 컨테이너 실행하는것이 좋을지
  - 결정된 worker node 정보를 갖고 api 가 해당 node의 kubelet에 접속
  - 컨테이너 실행해달라고 요청

  - kubelet이 docker에게 컨테이너 실행해달라고 요청

- kube-controller-manager

  - pod를 관찰하며 개수를 보장
  - 컨테이너가 실행되고 있는 worker node 하나가 갑자기 다운된 경우 해당 프로세스 다 사라져 버림
  - control-plane에 있는 controller가 파악한 후 api에게 요청
  - 다시 scheduler 통해서 필요한 곳에 컨테이너 다시 실행



2. worker node component

- kubelet
  - 모든 노드에서 실행되는 k8s 에이전트
  - 데몬 형태로 동작
- kube-proxy
  - k8s의 network 동작을 관리
- container runtime
  - 컨테이너 실행하는 엔진
  - 우리는 docker 사용



## 애드온 프로그램

- 필요하다면 추가로 집어넣어서 사용 가능
  - coreDNS는 기본으로 설치됨
  - weavenet은 kube 설치할 때 이미 했음

- 네트워크 애드온
  - CNI - weave, calico, flanld, kube-route
- Dns 애드온 - coreDNS

![png](/assets/img/yunju/k8s_architecture/kubernetes_dashboard.png)

- 대시보드 애드온
  - pods, depoyments. api 리소스

- 클러스터 로깅

  - 컨테이너 로그, k8s 운영 로그들을 수집해서 중앙화
  - ELK, EFK, DataDog

  

# 2. Namespace



## namespace란

- K8s api 종류 중 하나
- 클러스터 하나를 여러 개의 논리적인 단위로 나눠서 사용
- K8s 클러스터 하나를 여러 팀이나 사용자가 함께 공유
- 용도에 따라 실행해야 하는 앱을 구분할 때 사용



## 예시

- 원래는 control-plane 1개 worker node 2개
- 근데 api 기준으로 클러스터 여러개 있는 것 처럼

``````shell
$ kubectl create namespace -hs
$ kubectl create namespace -ds
$ kubectl create namespace -dutyfree
``````

- Hs, ds, dutyfree 이라는 각각 namespace 만들어짐
  - (API, pod, service ...)
  - A 그룹의 홈쇼핑에서 운영되는 pod는 다 hs namespace 에서 운영
  - A 그룹의 백화점에서 운영되는 pod는 다 ds namespace 에서 운영
  - A 그룹의 면세점에서 운영되는 pod는 다 dutyfree namespace 에서 운영
- 실제 장비는 하나지만 시스템이 따로 있는 것 처럼 논리적으로 나눠서 쓸 수 있게하는 것 - namespace



## 장점

``````shell
$ kubectl get pod
``````

- 해당 namespace에서 동작중인 애플리케이션만 분류시켜서 볼 수 있음

- 수많은 pod들이 동작되는데 분류하여 관리가능

``````shell
$ kubectl create deploy ui --image=nginx --namespace hs
$ kubectl create deploy ui --image=nginx --namespace ds
$ kubectl create deploy ui --image=nginx --namespace dutyfree
``````

- 실제 만들어진 ui는 세개 



## 생성



### 1) CLI

``````shell
$ kubectl get namespaces
``````

- 현재 시스템이 namespace 몇개 있는지
- k8s에서 설치될 때 기본으로 4개 들어감 



``````shell
$ kubectl get pods -n hs
$ kubectl get pods --all-namespace
``````

- Hs namespace 에 있는 pod 보여줘라
- 싹다 보여줘라



```shell
$ kubectl create namespace blue
```



- blue namespace 생성





### 2) yaml

``````shell
$ kubectl create namespace orange --dry-run -o yaml > orange-ns.yaml
``````

- 실제로 만들진 말고 만들 수 있는지 확인만
- 그 결과를 yaml파일 (orange-ns.yaml) 로 보여줘

![png](/assets/img/yunju/k8s_architecture/orange-ns.png)



``````shell
$ kubectl create -f orange-ns.yaml
``````

![png](/assets/img/yunju/k8s_architecture/namespaces.png)

- yaml파일은 시스템 안에 항상 존재하기 때문에 언제든 만들 수 있음



## 실행

``````shell
$ kubectl craete -f nginx.yaml -n blue
``````

- Nginx.yaml를 blue namespace에서 실행

![png](/assets/img/yunju/k8s_architecture/get_namespace.png) 



## switch

- 기본으로 사용하는 namespace를 default가 아닌 다른 이름의 namespace로 switch
- namespace를 포함한 context 등록

``````shell
$ kubectl config set-context blue@kubernetes --cluster=kubernetes --user=kubernetes-admin --namespace=blue
$ kubectl config view
``````

![png](/assets/img/yunju/k8s_architecture/config_view.png)

- 등록된 namespace로 context 변경

``````shell
$ kubectl config current-context
$ kubectl config use-context blue@kubernetes
``````

![png](/assets/img/yunju/k8s_architecture/current_context.png)



- default namespace 안에서 동작중인 mypod라는 pod 삭제

``````shell
$ kubectl delete pods mypod -n default
``````



- 원래로 돌려 놓기

``````shell
$ kubectl config use-context kubernetes-admin@kubernetes
``````



## 삭제

- namespace 지워버리면 안에있는 리소스 싹 다 날라감

``````shell
$ kubectl delete namespaces blue
``````





# 3. yaml 템플릿



## yaml

- 사람이 쉽게 읽을 수 있는 데이터 표현 양식
- 띄어쓰기가 굉장히 중요
- top-down 방식 아님
- k8s용 문법

## 기본문법

- Python처럼 들여쓰기로 데이터 계층 표기
- 들여쓰기 할때는 tab이 아닌 space bar 사용
- Scalar 문법 : ":"를 기준으로 key: value 설정
- 배열 문법 : "-" 문자로 여러개 나열
