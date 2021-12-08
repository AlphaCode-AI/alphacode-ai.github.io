---
layout: post
published: true
title: "[Kubernetes] Pod"
Date: 2021-12-08
tags: [study,docker,Kubernetes,Pod]
author: cindy
---
# 파드(Pod)
- Container 와 Pod 개념
  - container
  
  ![png](/assets/img/Cindy/pod/pod_1.png)

  - Pod 란? 
    - 컨테이너를 표현하는 K8S API의 최소 단위, 다시말해 컨테이너를 동작시켜주는 최소 단위
    - Pod 에는 하나 또는 여러개의 컨테이너가 포함될 수 있음
  
  ![png](/assets/img/Cindy/pod/pod_2.png)

***1. Pod 생성하기 (2가지 방식)***

![png](/assets/img/Cindy/pod/pod_3.png)

> 실습
  - CLI으로 pod 생성
  
![png](/assets/img/Cindy/pod/pod_4.png)

***2. multi-container Pod 생성하기***

![png](/assets/img/Cindy/pod/pod_5.png)

> 실습
  - yaml로 pod 생성
  - 하나의 pod에 centos 와 webserver 컨테이너가 따로 존재
  
![png](/assets/img/Cindy/pod/pod_6.png)

  - ip 가 동일하게 공유된다.
  
![png](/assets/img/Cindy/pod/pod_7.png)

# 파드(Pod)의 동작 flow
- pending : schduler가 worker node들 중 어느곳에 webserver 을 실행시킬지 선택하는 과정을 일컫는다.

![png](/assets/img/Cindy/pod/pod_8.png)

> Question & Answer
- dry -run 옵션 : 실행하지말고 실행이 되는지까지만 확인한다.
```bash
# 컨테이너 image 'redis123'을 실행하는 pod 'redis'를 yaml을 이용해 생성하시오.
# 앞서 만든 redis pod의 image를 redis로 수정하여 동작시키시오.
$ kubectl run redis --image=redis123 --dry-run -o yaml > redis.yaml
# yaml 파일 하나가 생김
$ kubectl create -f redis.yaml
$ kubectl get pods redis
# 에러 발생
$ kubectl describe pod
$ kubectl edit pod redis
$ kubectl get pods 
```
![png](/assets/img/Cindy/pod/pod_10.png)

![png](/assets/img/Cindy/pod/pod_11.png)

![png](/assets/img/Cindy/pod/pod_12.png)

# livenessProbe를 이용해 self-healing Pod (kubelet으로 컨테이너 진단하기)
- kubernetes Feacures - self-healing
  - Restarts containers that fail, replaces and reschedules containers when nodes die, kills containers that don't respond to your user-defined health check, and doesn't advertise them to clients until they are ready to serve.

- 주기적으로 상태를 확인한다.  

![png](/assets/img/Cindy/pod/pod_13.png)

![png](/assets/img/Cindy/pod/pod_14.png)

![png](/assets/img/Cindy/pod/pod_15.png)
  - failureThreshold : health check 실패 횟수

# init container를 적용한 Pod
- 앱 컨테이너 실행 전에 미리 동작시킬 컨테이너
- 본 Cantainer가 실행되기 전에 사전 작업이 필요한 경우 사용
- 초기화 컨테이너가 모두 실행된 후에 앱 컨테이너 실행
- https://kubernetes.io/ko/docs/concepts/workloads/pods/init-containers/ (예제 제공하고 있음)

![png](/assets/img/Cindy/pod/pod_16.png)

> 실습

![png](/assets/img/Cindy/pod/pod_17.png)

![png](/assets/img/Cindy/pod/pod_18.png)

![png](/assets/img/Cindy/pod/pod_19.png)

# infra container(pause) 이해하기
- pod가 생성될 때 함께 생성되며, pod의 ip, hostname 와 같은 정보를 관리해 주는 컨테이너이다.
  
![png](/assets/img/Cindy/pod/pod_20.png)

# static Pod 만들기
- API 서버없이 특정 노드에 있는 kubelet Daemon에 의해 동작에 의해 직접 관리
- /etc/kubernetes/manifests/ 디렉토리에 k8s yaml 파일을 저장 시 적용됨

![png](/assets/img/Cindy/pod/pod_21.png)

```bash
# API 도움없이 kubelet Daemon으로 Pod 실행
# static Pod path로 정의
# 정의된 곳에 Pod yaml를 넣어주면 동작한다.
$ cat /var/lib/kubelet/config.yaml 
```
---
# Souce
https://www.youtube.com/watch?v=0rYt3PcggzA&list=PLApuRlvrZKohaBHvXAOhUD-RxD0uQ3z0c&index=10

https://www.youtube.com/watch?v=ChArV14J6Ek&list=PLApuRlvrZKohaBHvXAOhUD-RxD0uQ3z0c&index=11

https://www.youtube.com/watch?v=-NeJS7wQu_Q&list=PLApuRlvrZKohaBHvXAOhUD-RxD0uQ3z0c&index=12

https://www.youtube.com/watch?v=ChArV14J6Ek&list=PLApuRlvrZKohaBHvXAOhUD-RxD0uQ3z0c&index=13

https://www.youtube.com/watch?v=qEu_znIYCz0&list=PLApuRlvrZKohaBHvXAOhUD-RxD0uQ3z0c&index=14