---
title: LNK
description: 
published: true
date: 2022-12-04T09:34:40.667Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:17:18.519Z
---

# LNK

LNK는 특정한 파일의 원본 경로를 가리키는 파일입니다. 해당 아티팩트는 Windows 운영체제에서만 존재합니다. UNIX 등의 환경에서는 In 명령어를 활용하여 유사한 기능을 구현할 수 있습니다. LNK 파일은 악용될 시 원본 정상 파일을 가리키는 것이 아니라, 악성 파일을 가리키도록 저장하고 있는 경로를 변화하여 정보 유출, 악성 프로그램 실행 등의 공격에 활용될 수 있습니다.

## 아티팩트 학습목표

분석자는 해당 아티팩트를 분석하여 바로가기 파일의 MAC 시간 정보, 원본 파일의 MAC 시간 정보, 크기, 경로, 파일이 생성된 볼륨의 Serial Number, 파일이 생성된 컴퓨터의 Machine ID 등을 알 수 있습니다. 이를 통해 분석 대상 컴퓨터에서 생성된 LNK 파일과 분석 대상 컴퓨터에서 생성되지 않은 LNK 파일을 구별 할 수 있으며, 정상 LNK 파일로 위조하고 있는 악성 파일을 식별할 수 있습니다.


## 관련 참조
기본 정보 : [Basic](/ko/Artifact/LNK/Basic)

추가 학습 정보 : [Advanced](/ko/Artifact/LNK/Advanced)