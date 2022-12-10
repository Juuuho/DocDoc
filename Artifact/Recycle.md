---
title: Recycle.bin
description: 
published: true
date: 2022-11-27T14:50:48.389Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:26:30.353Z
---

# Recycle.bin

Windows 운영체제에 있어 파일을 삭제할 경우, Shift + Del를 사용하여 삭제하지 않는 한 바로 삭제되는 것이 아니라 휴지통을 거쳐서 삭제되게 됩니다. 이 때, 파일이 기록된 위치의 변화는 없으며 단지 NTFS의 $MFT 영역에 있어 Entry의 정보 변화만이 일어납니다.

## 아티팩트 학습목표
분석자는 해당 아티팩트를 분석하여 사용자가 삭제한 파일들의 경로와 크기, 삭제 시각, 삭제된 파일의 원본 데이터를 알 수 있습니다. 이를 통해 사용자가 삭제한 파일을 복구할 수 있으며, 삭제된 파일에 대한 메타 데이터를 확인할 수 있습니다.


## 관련 참조
기본 정보 : [Basic](/ko/Artifact/Recycle/Basic)

추가 구성 정보 : [Advanced](/ko/Artifact/Recycle/Advanced)