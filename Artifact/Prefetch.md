---
title: Prefetch
description: 
published: true
date: 2022-11-27T14:11:42.894Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:20:30.002Z
---

# Prefetch

Prefetch란, “Pre” + “Fetch”의 의미 그대로 미리 불러오는 것을 의미합니다. 본 아티팩트는 Windows XP 부터 지원되기 시작하였습니다. 응용프로그램이 메모리에 적재될 때, 미리 필요로 하는 파일과 데이터를 불러와 시간의 소모를 줄이는 역할을 수행하는 Artifact입니다. 기본적으로 MAM 형태로 압축되어 있기에 그 내용을 확인하기 위해서는 압축을 해제하는 과정이 요구됩니다.

-   Prefetch는 가상 메모리를 활용하는 아티팩트이기에, 다른 응용 프로그램이 메모리를 요구할 때 Page - In, Page - Out 되는 문제가 발생합니다. 이러한 문제를 해결하기 위해서, Paging을 발생시킨 응용 프로그램이 종료되어 메모리가 해제될 시 Page - Out 된 응용 프로그램을 자동으로 메모리에 이동시키는 Superfetch 아티팩트가 등장하였습니다.

## 아티팩트 학습목표

분석자는 해당 아티팩트를 분석하여 응용 프로그램의 마지막 실행 시각, 실행 횟수, 실행 시 참조하는 라이브러리 목록과 참조 순서를 알 수 있습니다. 이를 통해 정상 파일로 위조하고 있는 악성 파일, 악성 파일을 참조하고 있는 응용 프로그램, 분석 시점에는 삭제된 응용 프로그램의 목록 등을 확인할 수 있습니다.

## 관련 참조
기본 정보 : [Basic](/ko/Artifact/Prefetch/Basic)

추가 학습 정보 : [Advanced](/ko/Artifact/Prefetch/Advanced)