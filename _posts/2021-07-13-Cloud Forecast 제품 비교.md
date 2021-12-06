---
layout: post
title: Cloud Forecast 제품 비교
published: true
date:   2021-07-13 00:00:00 +0900
feature-img: "assets/img/feature-img/totoro.png"
thumbnail: "assets/img/thumbnails/feature-img/totoro.png"
excerpt:
author: inseon
tags: [Test, inseonchoi, Markdown]
---

> **Cloud Forecast 제품 비교** 






#### GCP / AZURE / AWS

`비교해 봅시다.`

![Cloud Forecast Product Compared](/assets/img/pexels/cloud_compare.png "비교해 봅시다.")

도움이 되셨나요 ^^?



##### With Ubuntu

#### Configurate python environment 

```
#Update apt 
sudo apt update

#Install python 3.6
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.6

#Set default python version
sudo update-alternatives --config python3
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2

sudo update-alternatives --config python
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 2

#Install pip virtualenv
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
sudo pip install virtualenv
```


#### Clone this repogitory 

```
sudo apt-get install git
git clone https://github.com/MZCloudnoa/MakeAutoML.git
```


#### Activate new environment and install pakege

```
cd MakeAutoML
virtualenv --python=python3 .env
source .env/bin/activate
pip install -r requirements.txt
```


#### Make config dir and service account file

```
mkdir config
vi [**project id**]-service-account.json
```


#### Make data file (To be updated......) 

```
gsutil -m cp -r \
  "gs://makeautoml/data/" \
  .
```

#### Set expriment configuration using json file 

```
........
```

#### Run (To be updated.....)

```
python main.py
```

