---
title: Basic
description: 
published: true
date: 2022-12-02T11:10:25.065Z
tags: 
editor: markdown
dateCreated: 2022-10-27T07:44:05.128Z
---

# Powershell Log

Powershell은 명령줄 쉘(CommandLine Shell), 스크립팅 언어 및 구성 관리 프레임워크로 구성된 플랫폼 간 작업 자동화 솔루션입니다.
파워쉘은 Windows 위주로 개발되어 왔으나 최근에는 Linux 및 macOS에서 실행할 수도 있습니다. Windows 버전은 .NET 프레임워크 기반, 그 외 운영체제 버전은 .NET 코어 기반으로 개발이 되고 있습니다.


## Powershell 로그 분석의 필요성
파워쉘은 명령어 지원 외에도 COM(Component Object Model), WMI(Windows Management Instrumentation)을 지원하기 때문에 윈도우의 리소스에 접근하는 것이 용이합니다.
공격자들은 목표 시스템에서 이미 설치가 되어 있으며, 다양한 기능을 수행할 수 있는 파워쉘을 적극적으로 활용하여 공격을 수행하는 경우가 많습니다. 
이에 따라 디지털 포렌식의 침해사고 대응 관점에서는 수많은 공격자의 파워쉘 관련 공격 흔적을 볼 수 있기 때문에 공격자가 수행한 파워쉘 명령문을 해석하는 것은 공격자의 행위를 파악하는데 있어 중요합니다.

## Powershell 로그 

### ConsoleHost_history.txt

파워쉘에 입력한 명령어 로그는 기본적으로 Windows 7 운영체제에서는 남지 않지만, Windows 10이상의 운영체제에서는 다음 경로에 기록됩니다.

- **경로 : C:\Users\\{사용자}\AppData\Roaming\Microsoft\Windows\Powershell\PSReadLine\ConsoleHost_history.txt**

![powershell_ch.png](/powershell/powershell_ch.png)

ConsoleHost_history.txt에서는 위 그림과 같이 사용자가 실행했던 명령어를 그대로 저장합니다.

### Powershell 이벤트 로그 

Windows의 이벤트들은 default로 활성화 되어 있는 핵심적인 이벤트들이 있는 반면, 수동으로 활성화시켜 주어야 하는 이벤트들도 있습니다. 파워쉘의 이벤트들은 대부분 비활성화 된 상태로 있습니다. 파워쉘 스크립트 블록 로깅이라는 것을 활성화한다면 이벤트 로그 Powershell-Operational에서 파워쉘 명령어가 로깅되는 것을 확인할 수 있습니다. 

침해 사고 예방을 위해서 혹은 파워쉘 관련 명령어가 로깅되는 것을 확인하고 싶다면 **그룹 정책 편집기(gpedit.msc)** 에서 다음의 경로를 통해 Powershell 스크립트 블록 로깅과 Turn on Module Logging(모듈 로깅 사용) 옵션을 활성화합니다.

- **경로 : [로컬 그룹 정책 편집기] - [컴퓨터구성/사용자구성] - [관리 템플릿] - [Windows 구성 요소] - [Windows Powershell]**

![powershell_2.png](/powershell/powershell_2.png)

위 그림과 같이 활성화하면 상태가 사용으로 변경됩니다.

![powershell_4.png](/powershell/powershell_4.png)

위 그림과 같이 사용 옵션을 선택해줍니다. **로그 스크립트 블록 호출 시작/중지 이벤트** 옵션은 오른쪽 도움말과 같이 명령, 스크립트 블록 등의 호출이 시작 또는 중지될 때 이벤트를 추가적으로 기록합니다.

![powershell_3.png](/powershell/powershell_3.png)

Turn on Module Logging 옵션은 위 그림과 같이 하나 이상의 모듈에 대해 작성하여야 합니다. Windows Powershell 코어 모듈에 대한 로깅을 키려면 그림과 같이 입력해줍니다.

또한, 파워쉘 스크립트 블록 로깅 활성화는 아래 레지스트리의 키 경로에서 **EnableScriptBlockLogging** 이라는 값을 1로 추가 혹은 수정해도 변경됩니다.

- **HKLM\SOFTWARE\Policies\Microsoft\Windows\Powershell\ScriptBlockLogging**


## Powershell 옵션 및 유틸리티 명령어

해당 절에서는 공격자가 자주 사용하는 파워쉘 옵션 및 유틸리티 정보를 알아보도록 하겠습니다.

공격자가 자주 사용하는 옵션을 다음 표로 정리하였습니다.

|Full 명령어|정보|실제 사용 keyword|
|-|-|-|
|ExecutionPolicy|실행 정책 값을 설정합니다.|-ep|
|EncodedCommand|인코딩된 값을 복호화합니다.|-enc|
|NonInteractive|파워쉘 프롬프트로 사용자의 입력을 받지 않습니다.|-noni|
|NoProfile|파워쉘 스크립트 실행 시 사용자의 프로필을 실행하지 않습니다.|-nop|
|WindowsStyle hidden|파워쉘 스크립트 실행 시 파워쉘 콘솔이 실행되지 않도록 합니다.|-w hidden|

이번에는 공격자가 자주 사용하는 파워쉘 유틸리티입니다.

|명령어|정보|short keyword|
|-|-|-|
|DownloadString|원격지의 파일 내용을 대상 시스템의 메모리에 복사합니다.|-|
|DownloadFile|리소스의 데이터를 로컬 파일로 다운로드합니다.|-|
|Invoke-Expression|로컬 컴퓨터에서 명령 또는 표현식을 실행합니다.|IEX|
|Invoke-Command|로컬 및 원격 컴퓨터에서 명령을 실행합니다.|ICM|
|New-Object|Microsoft .NET Framework 또는 COM 개체의 인스턴스를 생성합니다.|-|
|System.Net.Webclient|URL로 식별되는 리소스와 데이터를 주고 받기 위한 일반적인 메소드를 제공합니다.|-|


-**Example**

![powershell_5.png](/powershell/powershell_5.png)

예시로 사용한 명령어는 다음과 같습니다. 해당 코드가 실행되더라도 콘솔 창이 출력되지 않고(-w hidden) 사용자의 입력을 받을 필요가 없으며(-noni), 사용자의 프로필을 실행하지 않고 인코딩 된 명령문을 복호화(-enc)하는 것을 볼 수 있습니다.



## 추가 관련 참조
더 자세한 사항은 [Advanced](/ko/Artifact/Powershell/Advanced) 에서 확인하실 수 있습니다.
- 파워쉘 명령을 이용하여 작업 스케줄 생성 및 분석 :  [TaskScheduler using Powershell](/ko/Artifact/Task_Scheduler/Advanced)
- 참고 출처 : https://learn.darungrim.com/contents/powershell-security-basics-01.html
- 참고 출처 : http://blog.plainbit.co.kr/archives/3764