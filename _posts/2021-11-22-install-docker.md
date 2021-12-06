---
layout: post
published: true
title: "Install Docker Engine on Ubuntu"
date: 2021-11-22
excerpt: "How to install docker engine on Ubuntu"
tags: [Docker, ai center]
author: emma
---

[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu "Install Docker Engine on Ubuntu")

## Prerequisites
### OS requirements (21.11.22)
To install Docker Engine, you need the 64-bit version of one of these **Ubuntu** versions:

* Ubuntu Impish 21.10
* Ubuntu Hirsute 21.04
* Ubuntu Focal 20.04 (LTS)
* Ubuntu Bionic 18.04 (LTS)

![os requirements](/assets/img/install-docker/install_docker1.png)

## Install using the repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.
     

### Set up the repository
1. Update the apt package index and install packages to allow apt to use a repository over HTTPS:

    ![set up repository](/assets/img/install-docker/install_docker2.png)
    ![set up repository](/assets/img/install-docker/install_docker3.png)

2. Add Docker's official GPG key
   
    ![set up repository](/assets/img/install-docker/install_docker4.png)

3. Set up the repository
    ![set up repository](/assets/img/install-docker/install_docker5.png)

### Install Docker Engine
1. Update the ```apt``` package index
    ![set up repository](/assets/img/install-docker/install_docker6.png)

2. Install the ***latest version*** of Docker Engine and containerd
    ![set up repository](/assets/img/install-docker/install_docker7.png)
    ...

3. Verify that Docker Engine is installed correctly by running the ```hello-world``` image.
    ![set up repository](/assets/img/install-docker/install_docker8.png)
