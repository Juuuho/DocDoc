---
title: MUICache
description: 
published: true
date: 2022-11-27T14:23:01.653Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:40:06.666Z
---

# MUICache

MUICache는 다중 언어를 지원하기 위해 프로그램 이름을 저장하는 기능을 수행합니다.

가령, Windows의 기본 프로그램인 PaintBrush, Regedit, notepad 등은 그림판, 레지스트리 편집기와 메모장 등으로 수정되어 표현됩니다.

이 외에도, 프로그램이 컴퓨터에서 한 번이라도 실행되었다면 사용자가 의도적으로 삭제할 때까지 응용프로그램 목록을 저장하고 있기에 프로그램의 실행 여부를 추적할 수 있습니다.

## 아티팩트 학습목표
분석자는 해당 아티팩트를 분석하여 응용 프로그램이 사용하는 국가 별 이름과 사용자가 사용한 응용 프로그램의 목록, 경로, 평소에 실행하지 않았던 프로그램 목록 등을 알 수 있습니다. 이를 통해 실행 후 삭제된 프로그램이나 공격자가 실행한 삭제 프로그램 등의 흔적을 찾을 수 있습니다.

## 관련 참조

기본 정보 : [Basic](/ko/Artifact/Registry/MUICache/Basic)

학습 정보 : [Advanced](/ko/Artifact/Registry/MUICache/Advanced)
