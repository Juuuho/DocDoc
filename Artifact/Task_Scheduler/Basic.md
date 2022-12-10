---
title: Basic
description: TaskScheduler_Basic
published: true
date: 2022-10-28T03:31:12.700Z
tags: taskschd.msc, schtasks, taskscheduler
editor: markdown
dateCreated: 2022-10-26T06:17:00.612Z
---

# Windows Task Scheduler

윈도우즈 작업 스케줄러는 특정 조건이 충족될 때마다 미리 정의된 작업이 자동으로 실행되도록 하는 Windows에 포함된 기능입니다.

사용자가 백업을 위해 프로그램이나 스크립트의 실행을 예약하거나, 특정 이벤트가 발생했을 때 이메일을 보내는 작업을 예약할 수 있습니다.

작업 스케줄러는 공격자 측면에서도 활용될 수 있습니다.
지속적인 공격을 위하여 악의적으로 작업 스케줄러에 작업을 예약하거나, 악성 행위를 하기전에 특정 이벤트를 수행하게 하거나 기다리게 할 수 있습니다.


## 저장경로

**Windows 10**

- **File System : %SystemDrive%\Windows\Tasks\\***

![탐색기tasks폴더.png](/task/탐색기tasks폴더.png)


- **Registry** 

**: HKLM\Software\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tasks\\***

![레지스트리task폴더.png](/task/레지스트리task폴더.png)

**: HKLM\Software\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree\\***

![레지스트리tree폴더.png](/task/레지스트리tree폴더.png)


## GUI


![실행.png](/task/실행.png)

[실행] 에서 taskschd.msc를 입력합니다.

![작업스케줄러gui.png](/task/작업스케줄러gui.png)

작업 스케줄러 프로그램을 GUI 환경에서 볼 수 있습니다.


### 기본 작업 스케줄 생성

GUI 화면에서 우측 상단에 있는 **기본 작업 만들기**를 선택하면 간단하게 기본 작업 스케줄을 생성할 수 있습니다.

![스케줄러_작성.png](/task/스케줄러_작성.png)

테스트를 위해 작업 스케줄을 생성합니다. 
**기본 작업 만들기** 화면에서는 스케줄에 대해 이름과 설명 작성이 가능합니다.
본 테스트에서 이름은 docdoc으로 작성합니다.

![스케줄러_트리거.png](/task/스케줄러_트리거.png)

**작업 트리거** 화면에서는 특정 작업에 대해 언제 스케줄을 실행할 것인지에 대해 지정합니다.

![스케줄러_트리거2.png](/task/스케줄러_트리거2.png)

해당 화면에서는 시작 시각 설정과 이전 그림에서 기간으로 지정했을 때 기간 설정을 할 수 있습니다.

![스케줄_특정_이벤트.png](/task/스케줄_특정_이벤트.png)

트리거 설정 화면에서 **특정 이벤트가 기록될 때** 옵션을 선택하면, 작업 스케줄을 실행하기 위해 필요한 이벤트 로그파일과 파일 내 이벤트 ID를 설정할 수 있습니다. 

![스케줄러_수행작업.png](/task/스케줄러_수행작업.png)

**동작** 화면에서는 트리거가 발생할 시 작업 스케줄에서 어떤 동작을 수행할 지 묻습니다.

현재 지원하는 기능은 사용자가 지정하는 프로그램/스크립트에 대한 시작 작업입니다.
전자 메일 보내기와 메세지 표시는 현재 사용 할 수 없습니다.

![스케줄러_프로그램_등록.png](/task/스케줄러_프로그램_등록.png)

**프로그램 시작** 화면에서는 작업 스케줄에 등록할 프로그램 혹은 스크립트를 지정할 수 있습니다.

![스캐줄러_등록_마침.png](/task/스캐줄러_등록_마침.png)

**요약** 화면에서는 지금까지 설정한 작업 스케줄을 보여줍니다.

![등록_후_이벤트로그1.png](/task/등록_후_이벤트로그1.png)

작업 스케줄을 생성하면 작업 스케줄러화면에서 확인 가능하며, 기록 탭에서는 TaskScheduler 이벤트로그에 등록된 작업이
기록되는 것을 확인할 수 있습니다.

> Windows-TaskScheduler-Operational에 작업 스케줄 이벤트가 기록되지 않는다면 이벤트 뷰어에서 **로그 사용**을 통해 활성화해야 로그가 기록됩니다.
{.is-info}


이벤트로그에서는 등록 시각과 이벤트 ID, 스케줄 등록 사용자, 작업 스케줄 이름 등을 알 수 있습니다.

![작업_생성_advance.png](/task/작업_생성_advance.png)

작업 스케줄러 메인 화면에서 기본 작업 만들기 밑에 있는 **작업 만들기** 를 누르면 더 상세하게 작업 스케줄을 작성할 수 있습니다.


## CLI

CMD, Powershell에서도 one line으로 작업 스케줄 작성이 가능합니다.

스케줄러 관련 명령어는 일반적으로 schtasks 명령어를 사용합니다.

|Parameter|Description|
|-|-|
|schtasks change|/tr : task를 실행하는 프로그램 <br> /ru : task가 실행되는 사용자 계정 <br> /rp: 사용자 계정 비밀번호 </br> /it : 작업에 대화형 전용 속성 추가
|schtasks create|새 작업 예약|
|schtasks delete|예약된 작업 삭제|
|schtasks end|작업에 의해 시작된 프로그램 중지|
|schtasks query|컴퓨터에서 실행하도록 예약된 작업 표시|
|schtasks run|예약된 작업 즉시 시작, 일정 무시|

다음으로 create 작업 시 매개변수로 올 수 있는 주요 명령어들은 다음과 같습니다.

|Parameter|Description|
|-|-|
|/sc|-MINUTE : 작업 실행 시간(분) 지정 <br> -HOURLY : 작업 실행 시간 지정 <br> -DAILY : 작업 실행 일 수 지정 <br> -WEEKLY : 작업 실행 주 수 지정 <br> -ONLOGON : 사용자가 로그온할 때마다 작업 실행 지정 <br> -ONIDLE : 시스템이 지정 시간동안 유후 상태일 때마다 작업 실행 지정
|/tn|작업 이름 지정(238자 이하, 공백 포함 시 따옴표 사용)|
|/tr|작업이 실행하는 프로그램 또는 명령 지정(262자 이하, default는 %Systemroot%\System32)|
|/s|원격 컴퓨터의 이름 또는 IP 주소 지정(default : Local)|
|/ru|지정된 사용자 계정 권한으로 작업 실행|
|/rp|기존 사용자 계정 암호 또는 매개변수로 지정된 사용자 계정 지정|
|/st|시간 형식으로 작업 시작 시간 지정(default : Local PC의 현재 시간)|

다음으로 query 작업 시 매개변수로 올 수 있는 주요 명령어들은 다음과 같습니다.

|Parameter|Description|
|-|-|
|/fo|출력이 표시될 형식을 지정합니다. <br> TABLE, LIST, CSV|
|/v|자세한 작업 출력을 표시합니다.


- **생성 Example**

![cmd_작업_생성.png](/task/cmd_작업_생성.png)

명령어를 통해서 GUI환경에서 만들었던 작업 스케줄을 동일하게 만들수있습니다.

- **삭제 Example**

![cmd_작업_삭제.png](/task/cmd_작업_삭제.png)

delete 명령으로 삭제가 이루어진 그림입니다. /F 명령어는 작업을 강제로 삭제하는 옵션입니다.

- **확인 Example**

![cmd_작업_확인.png](/task/cmd_작업_확인.png)

query를 날려 현재 등록된 작업 스케줄을 확인할 수 있습니다.

/fo 명령어를 통해 LIST 형식으로 출력하고, /v 명령어를 통해 작업 스케줄 정보가 자세히 출력된 것을 확인할 수 있습니다.


## Advanced

더 자세한 사항은 [Advanced](/ko/Artifact/Task_Scheduler/Advanced)에서 확인하실 수 있습니다.


