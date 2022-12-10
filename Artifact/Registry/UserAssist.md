---
title: UserAssist
description: 
published: true
date: 2022-11-27T14:13:15.068Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:37:59.035Z
---

# UserAssist

![userassist1.png](/userassist/userassist1.png)

UserAssist란, 사용자가 최근에 실행한 프로그램, 자주 사용하는 프로그램의 목록, 마지막 실행 날짜, 실행 횟수 등이 기록되어 있는 아티팩트입니다.  해당 아티팩트는 레지스트리에 속해 있습니다. 

유의해야할 사항은, 정보를 저장하는 방식입니다. 상단의 그림을 살펴본다면, 일반적으로 확인할 수 있는 응용 프로그램의 이름과는 상이함을 알 수 있습니다. UserAssist에서 관리하는 이름은 ROT-13 방식으로 Encoding되어 있기에 이름을 직관적으로 확인할 수 없습니다. 따라서 원본 데이터(응용 프로그램의 이름)을 확인하기 위해서는 ROT-13 방식으로 동일하게 Decoding 과정을 거쳐준다면 확인할 수 있습니다.

## 아티팩트 학습목표

분석자는 해당 아티팩트를 분석하여 최근 실행한 응용 프로그램의 이름, 응용 프로그램의 목록, 마지막으로 응용 프로그램을 실행한 시각, 응용 프로그램의 실행 횟수를 알 수 있습니다. 이를 통해 분석대상 PC에 존재하는 사용자가 실행한 응용프로그램 실행 횟수, 실행 시간 등을 Prefetch와 연관지어 확인할 수 있습니다.

## 관련 참조

기본 정보 : [Basic](/ko/Artifact/Registry/UserAssist/Basic)

추가 학습 정보 : [Advanced](/ko/Artifact/Registry/UserAssist/Advanced)