---
title: Windows 계정 생성 및 삭제
description: Windows 계정 생성 및 삭제
published: true
date: 2022-11-26T05:36:39.983Z
tags: 
editor: markdown
dateCreated: 2022-11-26T05:31:23.792Z
---

# Windows 계정 생성 및 삭제
- 분석 목적 : Windows 계정을 생성하거나 삭제했을 때 확인할 수 있는 흔적을 분석합니다.

- 분석 환경 : Microsoft Windows 10 Pro

- Analysis Files : 

|Analysis Files|Artifacts|
|-|-|
|Security.evtx|이벤트 로그|
|SAM|하이브 파일|

- 분석 도구

|도구|버전|URL|
|-|-|-|
|이벤트 뷰어|-|-|
|REGA|1.5.3.0|http://forensic.korea.ac.kr/tools.html|
|RegRipper|3.0|https://github.com/keydet89/RegRipper3.0|


- VM

|OS|버전|System Name|
|-|-|-|
|Microsoft Windows 10 Pro|10.0.19044 Build 19044.1826|DESKTOP-M3J1EH3|

## Windows 계정 생성
Windows 계정 생성 시에 확인할 수 있는 흔적 중 하나인 이벤트 로그와 레지스트리를 중점으로 분석합니다.

### 이벤트 로그 - ID: 4720
![4720_1.png](/windows계정/4720_1.png)

계정 생성과 관련된 이벤트 로그는 "C:\Windows\System32\winevt\Logs\Security.evtx"에서 확인 가능하며, 이벤트 뷰어에서는 "보안"에서 확인 가능합니다.
위 사진에서 확인할 수 있듯이 "주체:"에서는 계정을 생성한 계정의 정보가 있으며, "새 계정:"에서는 생성한 계정 정보인 Account1에 대한 정보가 저장되어 있습니다.

![4720_2.png](/windows계정/4720_2.png) 

또한 ID: 4720 보안 로그 이벤트에는 SAM 계정과 도메인 계정 모두 기록되며, 4722, 4738, 4732에도 계정 생성과 관련된 작업이 기록됩니다.

|Event ID|설명|
|-|-|
|4722|사용자 계정을 사용할 수 있습니다.|
|4738|사용자 계정을 변경했습니다.|
|4732|구성원을 보안된 로컬 그룹에 추가했습니다.|

4720 계정 생성 이벤트 로그 이후에 위의 표처럼 순서대로 계정 관련 작업이 기록됩니다.

### Registry - REGA
![rega_account2.png](/windows계정/rega_account2.png)

DFRC에서 제공하는 REGA 프로그램을 이용해서 확인한 계정 정보입니다. 생성한 계정 이름, SID, RID, LM 해쉬, NT 해쉬, 상태, 생성시각, 로그인시각 등을 확인할 수 있습니다.
이렇듯 계정과 관련된 다양한 정보를 확인할 수 있으며, 특히 계정 생성 시각을 통해 언제 계정이 생성되었는지 파악할 수 있습니다.


### Registry - RegRipper
![4720_생성_regripper1.png](/windows계정/4720_생성_regripper1.png)

SAM 하이브 파일을 RegRipper를 사용해서 확인한 생성 계정(Account2)에 대한 정보입니다. 생성한 계정 정보인 Username, SID, 계정 생성 시각, 마지막 로그인 날짜, 패스워드 Reset 날짜, 패스워드 입력 실패 날짜, 로그인 횟수 등을 확인할 수 있습니다.
이렇듯 계정과 관련된 다양한 정보를 확인할 수 있으며, 특히 계정 생성 시각을 통해 언제 계정이 생성되었는지 파악할 수 있습니다.

### 생성된 계정 폴더 확인
![삭제_전.png](/windows계정/삭제_전.png)

계정을 생성한 이후 생성한 계정(Account2)에 로그인하면, 위 사진과 같이 해당 계정의 폴더가 생성됩니다.


## Windows 계정 삭제
Windows 계정 삭제 시에 확인할 수 있는 흔적 중 하나인 이벤트 로그를 중점으로 분석합니다.

### 이벤트 로그 - ID: 4726
![4720_삭제_2.png](/windows계정/4720_삭제_2.png)

계정 삭제와 관련된 이벤트 로그 또한 "C:\Windows\System32\winevt\Logs\Security.evtx"에서 확인 가능하며, 이벤트 뷰어에서는 "보안"에서 확인 가능합니다.
위 사진에서 확인할 수 있듯이 "주체:"에서는 계정을 삭제한 계정의 정보가 있으며, "대상 계정:"에서는 삭제된 계정 정보인 Accout1에 대한 정보가 저장되어 있습니다.

![4720_삭제_1.png](/windows계정/4720_삭제_1.png)

또한 ID:4726 보안 로그 이벤트에는 SAM 계정과 도메인 계정 모두 기록되며, 4798, 4733, 4729에도 계정 삭제와 관련된 작업이 기록됩니다.

|Event ID|설명|
|-|-|
|4798|사용자의 로컬 그룹 구성원이 열거되었습니다.|
|4733|구성원을 보안된 로컬 그룹에서 제거했습니다.|
|4729|구성원을 보안된 글로벌 그룹에서 제거했습니다.|

4726 계정 삭제 이벤트 로그 이전에 위의 표처럼 순서대로 계정 관련 작업이 기록됩니다.

### 삭제된 계정 폴더 확인

![삭제_후.png](/windows계정/삭제_후.png)

생성 이후 삭제를 진행하더라도 계정의 폴더가 그대로 남아 있고, 해당 정보를 통해 계정 정보가 존재했다는 것을 확인할 수 있습니다.

## 마무리
계정 생성 및 삭제 후 확인할 수 있는 Windows상에 존재하는 흔적을 분석했습니다. 해당 분석 내용을 활용할 수 있는 예시로 RDP 침해사고를 들 수 있습니다. RDP 침해사고는 "원격 데스크톱 연결"을 통해 피해자 PC로 접속하여 새로운(피해자가 생성하지 않은) 계정을 생성해 악성코드를 다운받고, 실행할 수 있습니다. 만약 이러한 상황에서는 계정 생성 및 삭제 시간을 통해 공격자의 행동범위를 유추할 수 있습니다.


## 추가 관련 참조
- Evtx: [Evtx](/ko/Artifact/Evtx)
- Registry: [Registry](/ko/Artifact/Registry)
