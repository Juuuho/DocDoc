---
title: File System
description: 
published: true
date: 2022-11-27T15:02:21.382Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:27:23.672Z
---

# File System

파일 시스템이란 파일의 이름을 지정하고, 저장 및 검색을 위해 논리적으로 어디에 위치 시켜야 하는지에 대한 방법을 구성한 시스템입니다.



## 아티팩트 학습목표
분석자는 해당 아티팩트를 분석하여 파티션의 크기, Byte Per Sector, Sector Per Cluster, Serial Number,  OEM ID, Hidden Sector 포함 여부 등을 알 수 있습니다. 이 외에도, 파일 시스템의 구조 분석을 통해 파일 복구, 클러스터 할당 정보, 파일 삭제 여부, 비할당 영역 여부 등을 확인할 수 있습니다. 이를 통해 분석 대상 PC에서 삭제된 파일을 복구하고 사용자가 사용한 파일의 변경 정보도 확인할 수 있습니다. 


## 관련 참조
현재는 NTFS 정보만 작성되어 있습니다.

NTFS 기본 정보 : [Basic](/ko/Artifact/FileSystem/NTFS/Basic)

NTFS 추가 학습 정보 : [Advanced](/ko/Artifact/FileSystem/NTFS/Advanced)