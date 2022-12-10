---
title: Basic
description: 
published: true
date: 2022-12-02T11:00:44.462Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:54:59.648Z
---

# Amcache
Amcache는 최근 실행한 프로그램 정보를 가지고 있습니다. 예로, 실행파일의 경로, TimeStamp(마지막 변경시간, 생성시간), PE 헤더 data, 파일정보에 대한 정보를 가지고 있습니다. Windows 7에서는 RecentFileCache.bcf 파일로 존재했으나 하이브 파일인 Amcache.hve로 대체되었습니다.

정리하자면 Amcache.hve 파일은 프로그램 호환성 관리자와 관련된 레지스트리 하이브 파일로 응용 프로그램의 실행정보를 저장하며, 해당 정보를 통해서 응용 프로그램의 경로, 최초 실행시간, 삭제시간을 확인할 수 있습니다.

## Amcache 포렌식 관점
- 최초 실행시간, 삭제시간 확인
- 실행파일 경로 확인
- 안티 포렌식 프로그램 식별
- 외부 저장장치 흔적 확인

## Amcache 유의사항
- 'ProgramDataUpdater' 수행될 때마다 초기화(기본적으로 하루에 한 번 수행)
- 'ProgramDataUpdater'의 수행시간을 늘리거나 초기화 이전에 하루마다 backup을 진행

## Amcache 수집
Amcache 분석을 진행하기에 앞서 해당 파일의 경로를 파악하고 파일을 수집해야 합니다.

![amcache_path.png](/amcache/amcache_path.png)
Amcache.hve 경로는 "C:\Windows\appcompat\Programs"입니다. Amcache.hve 파일을 Export를 통해 수집합니다.


## Amcache 분석
Amcache 분석 방법에는 Encase, Rega, RegRipper(amcache 플러그인 사용), AmcacheParser 툴을 사용하는 방법이 있습니다. Amcache Parser를 사용하여 분석을 진행하는 것은 Advanced에서 자세히 설명하겠습니다.


## 추가 관련 참조
더 자세한 사항은 [Advanced](/ko/Artifact/Registry/AmCache/Advanced) 에서 확인하실 수 있습니다.
- Prefetch: [Basic](/ko/Artifact/Prefetch/Basic)
- Registry: [Registry](/ko/Artifact/Registry)