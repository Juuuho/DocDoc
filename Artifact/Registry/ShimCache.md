---
title: ShimCache
description: 
published: true
date: 2022-11-27T14:18:46.485Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:38:37.534Z
---

# Shimcache

Shimcache는 Windows 운영체제에서 응용 프로그램의 호환성을 관리하는 아티팩트입니다. 응용 프로그램에 호환성 문제가 발생한다면 해당 프로그램은 Shimcache에 기록됩니다. 

여타 아티팩트와 동일하게 원본 파일이 삭제가 되어도, 그 실행 기록을 확인할 수 있는 점에서 유의미한 아티팩트입니다. 특히, Shimcache와 유사하게 응용 프로그램의 실행 기록을 확인할 수 있는 Prefetch와는 달리 한 번 저장된 정보를 삭제하지 않고 관리하는 점이 특징입니다.

## 아티팩트 분석목표

분석자는 해당 아티팩트를 분석하여 호환성 문제가 발생한 응용 프로그램의 이름, 응용 프로그램의 경로, 응용 프로그램의 크기, 응용 프로그램의 마지막 실행 시각 등을 알 수 있습니다. 이를 통해 악성코드 실행 시 자주 발생하는 호환성 문제를 파악하여 침해사고 분석에 활용할 수 있습니다.

## 관련 참조

기본 정보 : [Basic](/ko/Artifact/Registry/ShimCache/Basic)

학습 정보 : [Advanced](/ko/Artifact/Registry/ShimCache/Advanced)