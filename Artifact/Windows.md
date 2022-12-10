---
title: Windows.edb
description: 
published: true
date: 2022-11-27T14:51:57.721Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:27:00.097Z
---

# Windows.edb
Windows.edb는 Windows Search index database입니다. 즉, 윈도우 검색기능을 위해 색인 정보가 들어있는 데이터베이스라고 할 수 있습니다. 인덱싱은 SearchIndexer.exe 프로세스에 의해 백그라운드에서 수행되며, 시스템에 파일이 많을수록 windows.edb 파일의 크기가 커집니다. 


## 아티팩트 학습목표
분석자는 해당 아티팩트를 분석하여 윈도우 파일 구조, 인터넷 접속 기록, 이메일 내용 등 윈도우 상에서 서치를 통해 인덱싱 된 모든 정보를 확인할 수 있습니다. 이를 통해 삭제된 레코드를 복구할 수도 있으며 사용자가 주고받은 이메일 내용, 파일 내용까지 확인할 수 있습니다.

## 관련 참조
기본 정보 : [Basic](/ko/Artifact/Windows/Basic)

추가 학습 정보 : [Advanced](/ko/Artifact/Windows/Advanced)