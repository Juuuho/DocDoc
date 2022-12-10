---
title: Basic
description: 
published: true
date: 2022-12-02T11:20:23.372Z
tags: 
editor: markdown
dateCreated: 2022-10-11T11:03:07.590Z
---

# 응용 프로그램 및 서비스 로그

응용 프로그램 및 서비스 로그에는 Windows 로그에 포함되는 Application, Security, Setup, System, Forwarded Events를 제외한 로그들이 기록됩니다.

응용 프로그램 및 서비스 로그에도 디지털 포렌식을 위해 사용되는 로그들이 많이 존재합니다.

예를 들면, 다음과 같습니다.

|주요 행위|관련 EventLog|
|-|-|
|장치(USB 등)|Partition\Operational, Storage-ClassPnP|
|원격 접속|TerminalServices-LocalSessionManager, RemoteConnectionManager|
|Powershell 명령 사용|Powershell\Operational, Powershell|
|작업 스케줄러|TaskScheduler\Maintenance|
|윈도우 디펜더|Windows Defender\Operational|


## 추가 관련 참조
더 자세한 사항은 [Advanced](/ko/Artifact/Evtx/응용프로그램및서비스로그/Advanced/) 에서 확인하실 수 있습니다. 해당 페이지에서는 위에 언급된 행위와 이벤트 로그 위주로 설명되어 있음을 참고바랍니다.

