---
layout: post
published: true
title: "[Docker] docker container"
date: 2021-11-24
excerpt: "docker container에 대해 알아보기"
tags: [Docker, Container, megazone, ai center]
author: yunju
---



# docker container




## 컨테이너
- 실행의 독립성을 확보해주는 운영체계 수준의 격리 기술
- 하나의 application 이라고 볼 수 있음



### 특징
- 여러개의 컨테이너가 완전하게 독립된 공간으로 분리되어 동작
	- cpu, memory, disk 등의 HW 리소스
	- 각 app의 user id
	- Network
- 하나의 컨테이너(application)에 수정사항 생길 경우 해당 컨테이너만 수정 후 배포 가능




### Docker host
- docker daemon이 동작하는 linux kernel 이 있는 시스템
- daemon
	- 항상 실행되고 있는 프로그램 (ex - server)
	- ls, rm -> daemon 아님
- 즉 docker가 container를 실행할 수 있는 플랫폼



### container
- 독립된 여러개의 컨테이너들은 하나의 docker daemon 하에 동작
- 각각의 컨테이너들은 완벽하게 독립되지만 커널은 하나 -> 각 컨테이너는 동일한 커널 사용
- docker host 입장에서 컨테이너는 단순히 동작되는 하나의 프로세스 
- 사용자 입장에서 컨테이너는 완전히 독립되어 동작하는 application으로 관리



## 구조


### 이미지
- 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것
- 상태값을 갖지 않고 변하지 않는다.



### 컨테이너 구조
- nodejs web application이 담긴 컨테이너라고 가정
![png](/assets/img/yunju/docker_container/container_image.png)


- 3번 layer
	- base image layer
	- app.js가 실행되기 위해 필요한 설치
- 2번 layer
	- source image layer
	- app.js 소스파일
- 1번 layer
	- app.js(application) 를 실행시키기 위한 방법들 나열
- 싹다 통틀어서 container image로 부름



### 컨테이너 이미지

- 컨테이너 이미지는 여러 layer 이미지로 구성됨
- 하나의 application이 잘 실행 될 수 있게 모아져 있는 이미지들의 조합
- 나중에 build 할 때 이 구조대로 build



### 컨테이너와 컨테이너 이미지의 차이
![png](/assets/img/yunju/docker_container/dockerhost.png)

- docker가 동작중인 docker host 내부
- 컨테이너 이미지가 하드디스크에 파일형태로 저장됨
- 각 레이어별로 파일이 따로따로 저장됨
- 이 컨테이너 이미지를 실행하면 메모리에 하나의 applicationn process로 running 중 -> container
- container 
	- 동작 중인 process
- container image
	- 파일



## 동작 방식
![png](/assets/img/yunju/docker_container/container_operation.png)

- $docker search nginx
	- docker hub에 nginx container image 있는지 search, 있으면 리스트로 뽑아줌
	- docker hub
		- 컨테이너 image를 저장해 놓고 있는 창고
		- 약 10만여개 넘게 있음
- $docker pull nginx:latest
	- 컨테이너 이미지 우리 hdd로 가져오기
	- 아직 컨테이너 아님
- $docker run --d --name web -p 80:80 nginx:latest
	- 컨테이너 이미지를 컨테이너화 시킴
	- docker 플랫폼 위에 하나의 컨테이너 동작
	- nginx라는 웹 서버가 웹이라는 이름오로 컨테이너 화 되어서 현재 실행중인 application 이 됨




## 실습
### 1. docker daemon이 동작중인지 확인
- $ systemctl status docker

![png](/assets/img/yunju/docker_container/systemctl.png)
- active(running) -> 현재 docker 잘 실행됨
- enabled -> 다음 부팅시에도 잘 실행됨



### 2. docker search

![png](/assets/img/yunju/docker_container/docker_search.png)
- 내가 원하는 container image가 docker hub에 있는지 search
- permission denied error
	- Got permission denied while trying to connect to the Docker daemon socket
	- 해당 문제는 사용자가 /var/run/docker.sock를 접근하려했지만 권한이 없어 발생하는 문제
	- 사용자가 root:docker 권한 갖고 있어야함
		- 사용자를 docker group 에 포함시키면 됨
		- sudo usermod -a -G docker $USER
		- 위 명령 후 시스템 재구동



### 3. image 확인
- $ docker images 
	- 현재 컨테이너 이미지 뭐 있는지 확인
	![png](/assets/img/yunju/docker_container/container_image_1.png)



### 4. image pull
- hub에 있는 컨테이너 이미지를 우리 시스템에 다운로드 
- $ docker pull nginx
- 아직 컨테이너 아님

![png](/assets/img/yunju/docker_container/docker_pull.png)



### 5. docker run
- 컨테이너 실행하고 확인
- 실행 후 뜨는 문자들 -> unique한 container id

![png](/assets/img/yunju/docker_container/container_run.png)

- 확인
![png](/assets/img/yunju/docker_container/check.png)

