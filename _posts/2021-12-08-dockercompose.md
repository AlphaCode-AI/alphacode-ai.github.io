---
Layout: post
Published: true
title: "[Docker] 빌드에서 운영까지 (using Docker compose)"
date: 2021-12-08
excerpt: "빌드에서 운영까지"
tags: [docker, docker compose, megazone, ai center]
author: debbie
---



# Docker Compose
 - 여러 컨테이너를 일괄적으로 정의하고 실행할 수 있는 툴
 - 하나의 서비스를 운영하기 위해서는 여러 개의 애플리케이션이 동작해야 함
 - 컨테이너화 된 애플리케이션 들을 통합 관리 가능

 

## Dockerfile 과 Docker-Compose
 - Dockerfile : 컨테이너 이미지를 생성하면서 특정 작업까지 같이 처리하게 해주는 도구
 - Docker-Compose : 다수의 컨테이너를 쉽게 실행할 수 있게 도와주는 도구

![png](/assets/img/yunju/dockercompose/docker.png)

## 실행
 - docker compose가 나를 대신해 여러 컨네이너들을 지정한 방식으로 실행
 - 컨테이너를 실행할 때 추가적인 모든 옵션들 yaml 파일 형태로 만듬



## command 와 비교

- command 
```shell
$ docker run --name db -v db_data:/var/lib/mysql --restart=always -e MYSQL_ROOT_PASSWORD=somewordpress mysql:5.7
```

- docker-compose.yml
```yaml
db:
    image: mysql5.7
    volumes:
        - db_data:/var/lib/mysql
    restart: always
    environment:
        MYSQL_ROOT_PASSWORD=somewordpress
```



## 기본 문법

```yaml
version: "3.9"

services:
	db:
		image: mysql:5.7
		volumes:
			- db_data:/var/lib/mysql
		restart: always
		environment:
			MYSQL_ROOT_PASSWORD: somewordpress
			MYSQL_DATABASE: wordpress
			MYSQL_USER: wordpress
			MYSQL_PASSWORD: wordpress
			
	wordpress:
		depends_on:
			- db
		image: wordpress:latest
		volumes:
			- wordpress_data:/var/www/html
		ports:
			- "8000:80"
		restart: always
		environment:
			WORDPRESS_DB_HOST: db:3306
			WORDPRESS_DB_USER: wordpress
			WORDPRESS_DB_PASSWORD: wordpress
			WORDPRESS_DB_NAME: wordpress
volumes:
	db_data: {}
	wordpress_data: {}
```

출처: https://docs.docker.com/samples/wordpress/

- Docker Compose를 사용하여 Docker 컨테이너로 구축된 격리된 환경에서 WordPress 실행하는 docker-compose.yml



### 1) image

- 실행할 이미지 지정



### 2) command

- 컨테이너에서 실행될 명령어 지정
- ex

```yaml
app:
	image: node:12-alpine
	command: sh -c "yarn install && yarn run dev"
```



### 3) link

- 다른 컨테이너와 연계할 때 연계할 컨테이너 지정

```yaml
webserver: 
	image: wordpress:latest
	link:
		db:mysql
```



### 4) ports

- 컨테이너가 외부에 공개하는 포트 지정
- "호스트머신의 포트번호:컨테이너의 포트번호"를 지정하거나 컨테이너의 포트번호만 지정
- 컨테이너의 포트번호만 지정한 경우 호스트 머신의 포트는 랜덤한 값으로 설정 됨

```yaml
ports:
	- "80"
	- "8443:443"
```



### 5) expose

- 링크 기능을 사용하여 연결하는 컨테이너에게만 포트를 공개할 때 expose 지정
- 호스트 머신에서 직접 액세스 하지 않고 웹 애플리케이션 서버 기능을 갖고 있는 컨테이너를 경유해서만 액세스 하고 싶은 경우 등에 사용

```yaml
expose:
	- "3000"
	- "8000"
```



### 6) depends_on

- 여러 서비스의 의존관계 정의
- Webserver 컨테이너 시작 전 db 컨테이너와 redis 컨테이너를 시작하고 싶을 때 아래와 같이 정의

``````yaml
services: 
	webserver:
		build:
		depends_on:
			- db
			- redis
	redis:
		image: redis
	db:
		image: postgres
``````

- 주의 : 컨테이너의 시작 순서만 제어할 뿐 컨테이너 상의 애플리케이션이 이용 가능해 질 때까지 기다리고 제어하지는 않음



### 7) volumes

- 컨테이너에 볼륨을 마운트

``````yaml
services:
	db:
		image: mysql:5.7
		volumes: 
			- db_data:/var/lib/mysql
volumes:
	db_data: {}
``````



### 8) environment

- 컨테이너에 적용할 환경변수 정의

``````yaml
database:
	image: mysql:5.7
	environment:
		MYSQL_ROOT_PASSWORD: pass
``````



### 9) restart

- 컨테이너가 종료될 때 적용할 restart 정책
- no: 재시작 되지 않음
- always: 컨테이너를 수동으로 끄기 전까지 항상 재시작
- On-failure: 오류가 있을 시에 재시작

``````yaml
database:
	image: mysql:5.7
	restart: always
``````



## docker compose로 컨테이너 실행

1. 서비스 디렉토리 생성
2. docker-compose.yml 생성
3. docker-compose 명령어



## docker-compose 명령어



### 1) up

- 컨테이너 생성 / 시작

- docker-compose.yml 을 기준으로 실행시켜 줘
- -d : detach모드 (background 모드) 로
- 다른 옵션 없을 경우 현재 있는 디렉토리에 있는 yml 파일 기준으로 실행

``````shell
$ docker-compose up -d
$ docker-compose -f /other-dir/docker-compose.yml
``````



### 2) ps

- 컨테이너 목록 표시

- 현재 docker-compose 에서 동작되고 있는 컨테이너 상태정보를 보여줘
- 현재 디렉토리에서 동작 중인 컨테이너만 확인 가능

``````shell
$ docker-compose ps
``````



### 3) scale

- 특정 컨테이너의 개수 조절
- mysql 컨테이너 2개

``````shell
$ docker-compose scale mysql=2
``````



### 4) down

- 현재 디렉토리에서 동작하는 컨테이너 종료

``````shell
$ docker-compose down
``````



### 5) rm

- 컨테이너 삭제



### 6) config

- 내가 만든 docker-compose.yml 파일에 오류없나 확인

``````shell
$ docker-compose config
``````



### 7) logs

- 컨테이너 로그 출력

``````shell
$ docker-compose logs webserver
``````



## 빌드에서 운영까지

1. 서비스 디렉토리 생성
2. 빌드를 위한 dockerfile 생성
3. Docker-compose.yml 생성 
4. docker-compose 명령어가 실행

​	-> docker-compose 명령어가 실행될 때 docker-compose.yml 에 있는 컨테이너가 dockerfile에 의해 build



 



