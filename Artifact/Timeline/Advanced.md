---
title: Advanced
description: 
published: true
date: 2022-11-29T07:19:33.085Z
tags: 
editor: markdown
dateCreated: 2022-11-12T14:34:45.776Z
---

# Timeline

- 분석 목적 : Windows Timeline 분석을 통해 얻을 수 있는 유의미한 정보를 확인합니다.

- 분석 환경 : Microsoft Windows 10 Pro

- Analysis Files : 

|Analysis Files|Artifacts|
|-|-|
|C:\Users\<profile>\AppData\Local\ConnectedDevicesPlatform\L.\<UserName>\ActivitiesCache.db|Timeline|

- 분석 도구 

|도구|버전|URL|
|-|-|-|
|DB Browser for SQLite|3.12.2.0|https://sqlitebrowser.org|
|WxTCmd|version 0.6.0.0|https://www.sans.org/tools/wxtcmd/|


## 상세분석
본 페이지에서는 Windows Timeline의 정보를 담고 있는 ActivitiesCache.db를 분석합니다. 포렌식 관점에서 중요한 테이블인 Activity, Activity_PackageID 테이블을 중점으로 분석하고, Timeline 관련 도구인 WxTCmD 사용법을 확인합니다.

## Activity
Activity 테이블은 Timeline DB에서 사용자의 행위, 파일 둥의 흔적을 분석하는데 유의미한 정보를 담고 있습니다. 본 단락에서는 표를 통해 Activity 테이블에서 얻을 수 있는 핵심 카테고리를 확인하고, 사용자의 행위에 따른 정보 추출 과정을 분석합니다. 아래 표는 Basic과 같이 확인하면 좋습니다.

|카테고리|설명|
|-|-|
|ID|Binary 값|
|AppID|Activity_PackageID 테이블의 패키지 ID|
|ActivityType|5 - 사용자가 응용 프로그램을 열음, 6 - 사용자가 다시 앱으로 작업 중, 10 or 16 - 복사/붙여넣기|
|ActivityStatus|1 - 활성, 2 - 업데이트됨, 3 - 삭제됨, 4 - 무시|
|TimeStamp|MAC Time이 UNIX Epoch 시간 형식으로 저장(Start - 활동이 시작된 시간, End - 활동이 종료된 시간, 마지막 수정 시간, 만료 시간 - 활동 만료 시간(생성 또는 수정 후 30일))|
|Payload|특정 작업 중에 사용자가 애플리케이션과 상호 작용한 것을 기록|
|PlatformDeviceId|NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\TaskFlow\DeviceCache와 연결|
|ETag|이벤트 순서를 지정|

### 사용자가 열었던 파일, 프로그램 확인
![timeline2222.png](/timeline/timeline2222.png)

![timeline4.png](/timeline/timeline4.png)

위의 그림들과 같이 AppActivityId 컬럼에 확장자 또는 파일명 검색을 통해 사용자가 열었던 파일을 확인할 수 있습니다.
또한 해당 파일을 열기 위해 사용하는 프로그램(ex. Microsoft.Office.WINWORD.EXE.15 등)을 AppID 컬럼에 필터링해도 같은 결과를 얻을 수 있습니다.

![timeline5.png](/timeline/timeline5.png)

![timeline6.png](/timeline/timeline6.png)

만약 exe 검색을 진행한다면 사용자가 열었던 프로그램을 확인할 수 있습니다. 검색 이후 TimeStamp의 MAC Time을 분석한다면 더욱더 상세한 분석이 가능합니다.


### 사용자의 복사/복사넣기 행위 확인
![timeline7.png](/timeline/timeline7.png)

![timeline8.png](/timeline/timeline8.png)

위 그림에서 ActivityType에 "16"을 필터링 한다면 사용자의 클립보드 사용(Copy/Snap) 이력을 확인할 수 있습니다. Payload 필드에 GDPR이 존재하며, ParentActivityId 필드에서 데이터가 복사된 애플리케이션의 ID를 확인 가능합니다.


## Activity_PackageID
![timeline1111.png](/timeline/timeline1111.png)

Activity_PackageID 테이블에서는 어플리케이션의 파일 이름, 실행 파일 경로, 기록 만료 시간을 저장합니다. 만료 시간은 마지막 응용 프로그램 사용시간부터 30일로 설정됩니다. Timezone은 UNIX epoch 시간 형식(1970년 1월 1일 자정 이후)으로 저장됩니다. 중요한 것은 디스크에 존재하지 않은 파일도 타임라인이 기록된 이후라면 타임라인에 보존됩니다. 포렌식 관점에서 현재는 존재하지 않지만 특정 시간에 파일이 존재하고, 작동했다는 것을 증명할 수 있습니다.

## ActivityOperation
ActivityOperation 테이블은 Activity 테이블과 구성이 크게 다르지 않습니다.

![timeline9.png](/timeline/timeline9.png)

구성이 크게 다르지 않지만 위의 그림처럼 Payload 컬럼에서 해당 파일의 "displayText"를 확인할 수 있습니다.

![timeline10.png](/timeline/timeline10.png)

displayText는 위의 그림처럼 Windows Timeline(Windows 로고 + Tab)의 기록 중 화면에 표시되는 문자(파일명, 프로그램명 등)을 기록하고 있습니다.

## WxTCmd 도구 사용
마지막으로 Timeline 분석 도구인 WxTCmd 도구 사용법을 확인해 보겠습니다.

![wxtcmd1.png](/timeline/wxtcmd1.png)

- 명령어: WxTCmd.exe -f "ActivitiesCache.db 경로" --csv "결과물 저장 경로"
위 명령어를 입력하여 해당 도구를 사용한다면 아래와 같이 csv 파일 3개를 확인할 수 있습니다.

![wxtcmd2.png](/timeline/wxtcmd2.png)

파일명은 "날짜_프로필_테이블.csv"로 저장되며, 순서대로 Activity, Activity_PackageIDs, ActivityOperations 테이블을 파싱한 결과입니다. 확인할 수 있는 내용은 "DB Browser for SQLite"를 이용해 db를 직접 확인한 결과와 같습니다. 분석 방법은 앞서 설명했던 ActivitiesCache.db 분석 방법, Basic의 내용을 참고합니다.


## 마무리
- Timeline 관련 분석 방법, 분석 내용을 확인해 봤습니다. Windows Timeline 기록에는 분석 시점에서 존재하지 않는 분석 대상 PC의 파일, 프로그램이 기록되어 있을 가능성이 존재합니다. 해당 정보를 찾는다면 MAC Time, displayText, 만료일 등을 추가적으로 파악하여 디지털 포렌식적으로 의미 있는 데이터를 얻을 수 있습니다.

## 추가 관련 참조
- IstroSec - How to trace the user: Windows 10 Timeline: https://www.istrosec.com/blog/windows-10-timeline/