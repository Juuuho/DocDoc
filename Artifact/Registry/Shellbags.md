---
title: Shellbags
description: 
published: true
date: 2022-11-27T14:14:36.010Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:37:21.953Z
---

# ShellBags
Windows 운영체제에서 Local, Remote에서 사용자가 접근한 폴더 정보를 기록하고 있습니다. 다만 이 때, 사용자가 GUI 방식으로 접근하지 않고 CMD나 API 등의 방법들로 폴더에 접근하였다면 ShellBag에 기록되지 않습니다. 예를 들면, CMD의 cd 명령어를 통해 폴더에 접근했다면 ShellBag에 기록되지 않습니다. 원본 폴더가 삭제되더라도, Registry의 MRU에 기록된 정보는 삭제되지 않으므로 사용자가 접근했던 폴더에 대한 정보를 확인할 수 있습니다.


## 아티팩트 학습목표

분석자는 해당 아티팩트를 분석하여 접근한 폴더의 목록, 폴더 접근 순서, 폴더의 마지막 접근에 관한 시간 정보, 폴더의 MAC 시간 정보, MFT Entry Number, 폴더의 이름, 폴더의 GUID 등을 알 수 있습니다.이를 통해 분석 대상 PC에 존재하는 사용자의 특정 폴더 접근 시간 정보나 존재하는 폴더의 삭제/덮어쓰기에 대한 흔적을 추적할 수 있습니다.

## 관련 참조
기본 정보 : [Basic](/ko/Artifact/Registry/Shellbags/Basic)

추가 학습 정보 : [Advanced](/ko/Artifact/Registry/Shellbags/Advanced)