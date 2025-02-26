---
title: 'AI 기반 광고 제작 효율성 향상 프로젝트: AlphaMistral 및 TTV 모델 활용 사례'
author: 정하성
date: "2025-02-26"
tags:
    - 생성형AI
    - 모델학습
    - 비디오캡셔닝
    - 백엔드
    - 데이터전처리
category: Tech
summary: 이 프로젝트는 생성형 AI 기술을 이용하여 광고 시안 제작을 신속하고 저렴하게 진행할 수 있는 서비스를 개발하는 것을 목표로 합니다. 이를 위해 sLLM(Small Large Language Model)과 TTV(Text To Video) 모델을 사용하여 콘티와 광고 영상을 자동 생성하는 시스템을 구축했습니다. 데이터 전처리 및 크롤링을 통해 모델을 학습하고, UI와의 상호작용을 가능하게 하는 백엔드 API를 개발했습니다. 결과적으로 광고 제작 과정의 Pre-Production 단계에서 시간과 비용 절감을 이뤄냈습니다.
layout: post
published: true
---

# AI 미디어 문화 향유 확산 사업 사례 소개
## 1. 개요
생성형 AI기술을 사용하여 영상광고 사전제작 서비스를 개발한 사례를 소개하고 주요 기능 및 프로젝트 성과를 설명합니다.

### 광고 제작 프로세스
(1) Pre-Production(최소 1주 ~ 1달이상 소요)

기획 > 스토리 보드 작성(1차) > 수정 요청 > 스토리 보드(2차) > 수정 요청 > 스토리 보드(3차) > 확인 작업 

(2) Main/Post-Production

촬영 > 편집

** 그림추가(아래 이미지참고)
![광고제작 프로세스 예시](../images/commercial_production_process.png)

### 기존 광고제작 과정의 문제점
- 광고 제작 단계 중 Pre-Production 단계는 창작자에게 많은 시간과 노력을 소모
- 실제 광고찰영 바로 전 단계이기 때문에, 수많은 논의와 수정을 거치고 시안삽화를 통한 장면묘사나 멘트 등 다양한 작업이 필요함
- 사전 제작 단계임에도 최소 수백만원에서 수억원까지 소요되는 비용이 발생

### 프로젝트 목표
생성형 AI모델을 개발 및 사용하여 광고 시안제작을 빠르게 할 수 있도록 지원하는 창작 보조 서비스를 개발 하는것을 목표로합니다.

## 2. 프로젝트 설명
### 프로젝트 워크플로우
(1) 자체 보유 스토리보드 데이터 제공 및 추가 관련 데이터 수집

(2) 스토리보드 및 영상 데이터 전처리

(3) 생성형 AI 서비스 모델 개발 및 파인 튜닝

(4) 영상 제작 지원 서비스 개발(인터페이스 및 DB)

(5) 서비스 실증 및 고도화

 ** 그림추가 (빨간 박스로 친 부분이 알파코드가 개발한 부분입니다.)
 ![프로젝트 워크플로우 예시](https://github.com/AlphaCode-AI/tech-docs/blob/main/images/service_layer.png)

### 모델학습용 데이터셋
** 아래 이미지 참조해서 새로 제작

![모델 학습 데이터셋 예시](../images/data.png)

### 콘티 제작용 sLLM(Small Large Language Model) 모델 개발
(1) 산업분류에 따른 제품 리스트 생성
- a.제공된 산업분류 기준으로 품목정의 (제품카테고리 정의)
- b.품목기준으로 표시기준에 해당하는 품질표기사항에 따라 정해진 제품유형 크롤링
  
(2) 제품유형에 따라 n개의 컷씬으로 cf 상황별 스크립트 데이터를 openai 통해 증강 (N=4~8로 진행)

(3) 증강한 데이터를 통해 AlphaMistral 파인튜닝 진행
* AlphaMistral은 Alphacode에서 자체 학습한 sLLM으로 Mistral7B 모델을 강화학습을 통해 한국어 답변의 정확성을 대폭 향상 시킨 모델입니다.
  해당 모델은 Open Ko-LLM LeaderBoard에서 한국어 기반 ​7B 모델 177개 중 1위(24년 7월 기준)

![리더보드1위 스크린샷](https://github.com/AlphaCode-AI/tech-docs/blob/main/images/leaderboard.png)

(4) 결과개선을 위해 프롬프트 파인튜닝 진행

sLLM 콘티 생성 결과

** 그림2
![모델 생성 콘티 예시](../images/sllm_storyboard_example.png)

### 영상 제작용 TTV(Text To Video) 모델 개발
(1) 크롤링한 50만건의 유튜브 영상에 대해 VLM(Vision Language Model), GPT를 이용해 Video Captioning 적용(데이터 전처리)
* Video Captioning: 비디오의 내용을 자동으로 분석하고, 이를 자연어로 설명하는 기술. 쉽게 말해, 영상에서 중요한 장면을 인식하고 이를 텍스트(자막)로 표현하는 과정

(2) 전처리한 데이터를 통해 TTV 모델학습

(3) 결과개선을 위해 프롬프트 파인튜닝 진행

(4) TTV모델이 일정한 결과를 출력하도록 고도화 진행중(25.02월 기준)


TTV모델 광고영상 생성 결과 (1)

<video width="100%" controls>
  <source src="../videos/tire.mp4" type="video/mp4">
</video>


TTV모델 광고영상 생성 결과 (2)

<video width="100%" controls>
  <source src="../videos/car_driving.mp4" type="video/mp4">
</video>

### 모델 서빙 API 개발
- UI와 상호작용하는 백엔드 서버 API 개발(네이버클라우드플랫폼)
- UI에서 API 호출시 콘티 및 영상등을 생성한 후 응답하는 구조

![백엔드 API 그림 예시](../images/backend_api.png)

## 3. 성과 및 효과
- 생성형 AI(AlphaMistral)를 통해 콘티작성 자동화
- 광고제작자들이 제안한 아이디어를 바탕으로 생성형 AI가 추가적인 아이디어를 생성하여 보다 풍부한 선택지 제공 
- 생성된 콘티 기반 스토리보드를 자동으로 빠르게 생성(TTV모델 이용)하여 제작시간 단축 
- 기존 프로세스에 비해 Pre-Production 제작 시간 대폭 감소 및 보다 적은 인력으로 제작 가능하여 인건비 등의 제작 비용 절감 


