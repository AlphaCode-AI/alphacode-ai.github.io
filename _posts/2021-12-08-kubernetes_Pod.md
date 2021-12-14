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

![png](/assets/img/Cindy/pod/pod_9.png)
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
- API 서버없이 특정 노드에 있는 kubelet Daemon 동작에 의해 직접 관리
- /etc/kubernetes/manifests/ 디렉토리에 k8s yaml 파일을 저장 시 적용됨

![png](/assets/img/Cindy/pod/pod_21.png)

```bash
# API 도움없이 kubelet Daemon으로 Pod 실행
# static Pod path로 정의
# 정의된 곳에 Pod yaml를 넣어주면 동작한다.
$ cat /var/lib/kubelet/config.yaml 
```

# Pod에 리소스(cpu, memory) 할당하기
- Resource Requests
  - 파드를 실행하기 위한 최소 리소스 양을 요청
- Resource Limits
  - 파드가 사용할 수 있는 최대 리소스 양을 제한
  - Memory limit을 초과해서 사용되는 파드는 종료(OOM kill)되며 다시 스케줄링 된다.
  - OOM killer : out of Memory Killer 의 약ㅏ로 메모리가 부족할 경우 프로세스를 강제로 종료시키는 역할
  - CPU는 Milicore단위 (1,000 Milicore = 1Core ) , Memory는 Mbyte 단위로 할당
- https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/
- 예를 들어 CPU request를 500ms로 하고, limit을 1000ms로 하면 해당 컨테이너는 처음에 생성될때 500ms를 사용할 수 있다. 그런데, 시스템 성능에 의해서 더 필요하다면 CPU가 추가로 더 할당되어 최대 1000ms 까지 할당될 수 있다.
![png](/assets/img/Cindy/pod/pod_28.png)

![png](/assets/img/Cindy/pod/pod_22.png)

![png](/assets/img/Cindy/pod/pod_23.png)

# Pod에 환경변수 설정하기

![png](/assets/img/Cindy/pod/pod_24.png)

> 실습

![png](/assets/img/Cindy/pod/pod_25.png)

![png](/assets/img/Cindy/pod/pod_26.png)

# Pod 구성 패턴의 종류

- Sidecar : App container에서 실행되는 log를 sidecar에 담아서 전송함.
- Adapter : 외부의 데이터를 Adapter로 끌고와 App container를 통해서 그래프를 표시하는 방식
- Ambassador : 데이터가 App container로 들어오면 해당 내용을 Ambassador가 받아 그 DB 자료를 다양한 곳으로 전송해주는 역할

![png](/assets/img/Cindy/pod/pod_27.png)

---
# Source
https://www.youtube.com/watch?v=0rYt3PcggzA&list=PLApuRlvrZKohaBHvXAOhUD-RxD0uQ3z0c&index=10

https://www.youtube.com/watch?v=ChArV14J6Ek&list=PLApuRlvrZKohaBHvXAOhUD-RxD0uQ3z0c&index=11

https://www.youtube.com/watch?v=-NeJS7wQu_Q&list=PLApuRlvrZKohaBHvXAOhUD-RxD0uQ3z0c&index=12

https://www.youtube.com/watch?v=ChArV14J6Ek&list=PLApuRlvrZKohaBHvXAOhUD-RxD0uQ3z0c&index=13

https://www.youtube.com/watch?v=qEu_znIYCz0&list=PLApuRlvrZKohaBHvXAOhUD-RxD0uQ3z0c&index=14

https://www.youtube.com/watch?v=lxCtyWPsb-0&list=PLApuRlvrZKohaBHvXAOhUD-RxD0uQ3z0c&index=15

https://www.youtube.com/watch?v=Uc-VnK19T7w&list=PLApuRlvrZKohaBHvXAOhUD-RxD0uQ3z0c&index=16

https://velog.io/@exljhun307/pod-%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98-%EC%84%A4%EC%A0%95%EA%B3%BC-%EC%8B%A4%ED%96%89-%ED%8C%A8%ED%84%B4

https://bcho.tistory.com/1291