---
title: AmCache
description: 
published: true
date: 2022-11-27T14:20:20.532Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:39:17.569Z
---

# AmCache

Amcache는 최근 실행한 프로그램 정보를 가지고 있습니다. 예로, 실행파일의 경로, TimeStamp(마지막 변경시간, 생성시간), PE 헤더 data, 파일정보에 대한 정보를 가지고 있습니다. Windows 7에서는 RecentFileCache.bcf 파일로 존재했으나 하이브 파일인 Amcache.hve로 대체되었습니다.

정리하자면 Amcache.hve 파일은 프로그램 호환성 관리자와 관련된 레지스트리 하이브 파일로 응용 프로그램의 실행정보를 저장하며, 해당 정보를 통해서 응용 프로그램의 경로, 최초 실행시간, 삭제시간을 확인할 수 있습니다.

## 아티팩트 학습목표

분석자는 해당 아티팩트를 분석하여 호환성 응용 프로그램의 이름, 응용 프로그램의 목록, 응용 프로그램의 경로, 파일의 최초 실행 시각, 파일의 삭제 시각, MAC 시간 정보, 최초 설치 시간 등을 알 수 있습니다. 이를 통해 ShimCache 아티팩트와 유사하게 침해사고 분석에 활용될 수 있습니다.

## 관련 참조

기본 정보 : [Basic](/ko/Artifact/Registry/AmCache/Basic)

학습 정보 : [Advanced](/ko/Artifact/Registry/AmCache/Advanced)