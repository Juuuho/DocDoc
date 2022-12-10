---
title: Basic
description: 
published: true
date: 2022-12-02T11:05:58.501Z
tags: 
editor: markdown
dateCreated: 2022-11-08T06:39:23.621Z
---

# Prefetch

Prefetch란, “Pre” + “Fetch”의 의미 그대로 미리 불러오는 것을 의미합니다. 본 아티팩트는 Windows XP 부터 지원되기 시작하였습니다. 응용프로그램이 메모리에 적재될 때, 미리 필요로 하는 파일과 데이터를 불러와 시간의 소모를 줄이는 역할을 수행하는 Artifact입니다. 기본적으로 MAM 형태로 압축되어 있기에 그 내용을 확인하기 위해서는 압축을 해제하는 과정이 요구됩니다.

-   Prefetch는 가상 메모리를 활용하는 아티팩트이기에, 다른 응용 프로그램이 메모리를 요구할 때 Page - In, Page - Out 되는 문제가 발생합니다. 이러한 문제를 해결하기 위해서, Paging을 발생시킨 응용 프로그램이 종료되어 메모리가 해제될 시 Page - Out 된 응용 프로그램을 자동으로 메모리에 이동시키는 Superfetch 아티팩트가 등장하였습니다.

프리패치의 시그니처는 "SCSA"(0x41434353) 입니다.

## Prefetch 설정

Prefetch는 다음 레지스트리 경로의 값에 따라 설정이 변경됩니다.

HKLM\\SYSTEM\\CurrentControlSet\\Control\\Session Manager\\Memory Management\\PrefetchParameters\\EnablePrefetcher

**0:** Prefetch 사용 안 함

**1:** ALP(Application-Launch Prefetching)만을 사용

\* ALP  : 자주 사용하는 응용프로그램의 정보를 Prefetch

**2:** BP(Boot Prefetching)만을 사용

\* BP :  부팅시 사용되는 파일이나 프로그램을 Prefetch

**3:** ALP와 BP 모두 사용

## Prefetch의 필요성

원본 프로그램이 삭제되어도 Prefetch에는 기록되어 있기에, 사용자의 사용 기록을 추적할 수 있고 이 외에도 악성 프로그램의 은닉 실행 기록등을 추적할 수 있습니다. 이 외에도, 정상 프로그램에 은닉되어 실행되는 Malware등의 실행 여부를 추적할 수 있습니다. 하지만 실제 발생하는 Malware 동작 대부분, 정상 프로그램의 이름과 경로로 위장하여 실행되기에 이름만으로는 추적하기 어렵다는 점을 숙지하여야 합니다.

## Prefetch의 경로

C:\\Windows\\Prefetch: Windows의 Prefetch 파일이 저장되어 있는 위치입니다.

![](/화면_캡처_2022-11-03_160449.png)

## Prefetch의 구성

다음 두 사진은 Prefetch 분석 도구인 WinPrefetchView의 분석 결과 일부 입니다.

![](/prefetch/prefetch1.png)

Nirsoft 사의 WinPrefetchView 분석 결과 (1)

![](/prefetch/prefetch2.png)

Nirsoft 사의 WinPrefetchView 분석 결과 (2)

Prefetch에서 확인할 수 있는 정보들은 다음 도표와 같습니다.

|     |     |
| --- | --- |
| Type | Meaning |
| File Name | 파일 이름 |
| Full Path | 파일의 경로 |
| Device Path | 응용 프로그램이 실행될 때 불러오는 라이브러리 등의 디바이스 경로를 나타냅니다. |
| Index | 응용 프로그램이 실행될 때 불러오는 라이브러리 등의 "순서"를 나타냅니다. |
| Run Counter | 실행 횟수 |
| Created Time | 실행 파일의 생성 시각 |
| Modified Time | 실행 파일의 수정 시각 |
| File Size | 실행 파일의 크기 |
| Last Run Time | 실행 파일의 마지막 실행 시각 |


## 관련 참조

https://doc.datg.xyz/ko/Artifact/Prefetch/Advanced

## 분석 도구

|도구|버전|URL|
|-|-|-|
|WinPrefetchView|1.3.7|https://www.nirsoft.net/|

## 추가 관련 참조
더 자세한 사항은 [Advanced](/ko/Artifact/Prefetch/Advanced) 에서 확인하실 수 있습니다.