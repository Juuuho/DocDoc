---
title: Basic
description: Windows.edb_Basic
published: true
date: 2022-12-02T10:54:06.852Z
tags: 
editor: markdown
dateCreated: 2022-11-22T07:03:59.402Z
---

# Windows.edb
Windows OS에서는 윈도우 검색(Windows Search)은 색인(Indexing)을 사용하여 빠르게 검색이 가능하도록 지원합니다. 기본적인 색인 대상은 MS사에서 제공하는 웹 브라우저(Edge, IE), 이메일(Outlook), 메신저 등이 있습니다

![windowsedb_path.png](/windowsedb/windowsedb_path.png)
파일명은 Windows.edb로 존재하며, 파일 경로는 "C:\ProgramData\Microsoft\Search\Data\Applications\Windows"입니다.

Windows.edb는 상시 사용이 진행중이기 때문에 해당 파일을 분석하기 위해서는 FTK Imager, Winhex64 등의 도구를 통해 파일을 Windows.edb 파일을 추출하거나 "윈도우 검색" 색인 기능을 off 시켜야 합니다. 상세한 분석 과정은 Advanced에서 진행합니다.

아래의 표는 Windows.edb가 가지고 있는 정보들입니다. Windows.edb는 색인 정보를 가지고 있는데 실제로 분석을 진행하면 Windows와 관련된 프로그램, 아티팩트 들의 로그기록과 같은 정보를 유지하고 있습니다.

|분석 데이터|획득 가능 정보|
|-|
|Outlook Mail Data|Time, To, From 등|
|OneNote Title|-|
|Internet History|IE, Edge 기록|
|Lnk list|-|
|Network Drive|-|
|Favorites|-|
|File, Folder Information|Time, Path 등|
|Activity History|Windows 10 Timeline|

위 표 이외에도 windows.edb는 다양한 정보를 가지고 있습니다. 

세부적인 내용은 Advanced에서 설명합니다.

## 추가 관련 참조
더 자세한 사항은 [Advanced](/ko/Artifact/Windows/Advanced) 에서 확인하실 수 있습니다.