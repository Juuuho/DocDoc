---
title: Advanced
description: TaskScheduler_Advanced
published: true
date: 2022-11-28T17:18:45.772Z
tags: powershell, taskscheduler, trigger, action, malicious, base64, eventlog
editor: markdown
dateCreated: 2022-10-26T06:17:26.588Z
---

# Windows Task Scheduler

Advanced에서는 Tasks 폴더에 남는 아티팩트를 분석하고, powershell을 통해 생성되는 작업 스케줄러를 분석하여 악의적인 스케줄은 없는지 파악해보는 내용이 작성되어 있습니다. 

- **분석 환경 : Windows 10 pro 21H2**
- **분석 대상 : %SystemDrive%\Windows\System32\Tasks\\**
- **분석 도구 :**

|도구|버전|URL|
|-|-|-|
|Visual Studio Code|1.72.2|https://code.visualstudio.com/|
|HxD|2.5.0.0|www.mh-nexus.de|



## 상세 분석

![tasks_폴더내_docdoc.png](/task/tasks_폴더내_docdoc.png)

%SystemDrive%\Windows\System32\Tasks 경로에서 Basic에서 생성했던 docdoc 작업 스케줄을 확인할 수 있습니다.

![docdoc_hex.png](/task/docdoc_hex.png)

작업 스케줄 아티팩트는 기본적으로 xml 형태로 저장됩니다. 인코딩 방식은 BOM(Byte Order Mark)이 FF FE로 존재하는 것을 봐서 UTF-16 LE(Little Endian) 방식이라는 것을 알 수 있습니다.


![docdoc_vscode3.png](/task/docdoc_vscode3.png)

본 분석에서는 Visual Studio Code로 진행합니다.

대부분의 파일 내 기록된 값은 작업 스케줄러 GUI 환경에서도 확인할 수 있습니다.

> 분석 대상 이미지에 작업 스케줄 파일이 남아있다면, 작업 스케줄러에서 **작업 가져오기**를 통해 GUI 환경에서 당시 사용자 or 공격자가 생성한 스케줄 내용에 대해 쉽게 확인할 수 있습니다. 
{.is-info}

또한, Settings에 있는 내용들은 GUI 환경에서 해당 작업 스케줄을 선택하여 우클릭 후 **속성**을 클릭하여 설정 값을 변경함에 따라 기록되는 정보가 달라집니다.

- **\<RegistrationInfo\>**

작업 스케줄 등록 시각과 등록 사용자, 그리고 작업 스케줄 이름에 대해서 알 수 있습니다.

-------

- **\<Triggers\>**

작업 스케줄의 트리거 정보를 나타냅니다.

![triggers.png](/task/triggers.png)

**StartBoundary**는 트리거에서 지정된 시각을 나타냅니다.
**Enabled**는 사용중인 상태라면 true를, 그렇지 않으면 false로 기록됩니다.
**DaysInterval**은 해당 작업 스케줄이 매일이기 때문에 1로 기록됩니다.

-------

- **\<Settings & IdleSettings\>**
 
 
![settings.png](/task/settings.png)


![docdoc_설정및조건.png](/task/docdoc_설정및조건.png)

작업 스케줄의 조건과 설정 정보를 나타냅니다.


**MultipleInstancesPolicy**는 작업이 이미 실행 중일때 어떤 규칙을 적용하느냐에 따라 값이 달라집니다.

|RULE|Information|
|-|-|
|IgnoreNew|default 값이며, 기존의 작업 인스턴스가 실행중일 때, 새 인스턴스를 시작하지 않습니다.|
|Parallel|작업 인스턴스를 병렬로 실행합니다.|
|Queue|새 인스턴스를 대기 시킵니다.|
|StopExisting|기존 인스턴스를 중지시킵니다.|

**DisallowStartIfOnBatteries**는 true일 때, 컴퓨터의 AC 전원이 켜져 있는 경우에만 작업을 시작하도록 합니다. 즉, AC 전원을 통해 PC나 노트북을 충전 중일때만 작업을 시작합니다.

**StopIfGoingOnBatteries**는 true일 때, 컴퓨터가 배터리 전원으로 전환되는 경우 작업을 중지합니다.

**AllowHardTerminate**는 true일 때, 요청할 때 실행중인 작업이 끝나지 않으면 강제로 작업을 중지합니다. windows terminateprocess api를 통해 종료되도록 설정되어 있습니다.

**StartWhenAvailable**은 true일 때, 예약된 시작 시간을 놓친 경우 가능한 대로 빨리 작업을 시작하도록 합니다. 기본값은 false입니다.

**RunOnlyIfNetworkAvailable**은 true일 때, 작업 스케줄 생성시 지정해 놓은 네트워크 연결이 수행되었을 때 작업을 시작하도록 하는 옵션입니다.

**Duration**과 **WaitTimeout**은 조건 탭에서 "컴퓨터가 다음 시간 동안 유휴 상태인 경우에만 작업 시작"이라는 옵션이 활성화되어있다면 기록됩니다.

**Duration**은 해당 시간 동안 유휴 상태인 경우를 의미하고,
**WaitTimeout**은 해당 시간 동안 유휴 상태 대기를 의미합니다.

**StopOnIdleEnd**는 컴퓨터의 유휴 상태가 끝나면 작업 실행을 중지한다는 것을 의미합니다.

**RestartOnIdle**은 유휴 상태가 재개되면 다시 시작한다는 것을 의미합니다.

**AllowStartOnDemand**는 true일 때, 요청 시 작업이 실행되도록 허용하는 것을 의미합니다.

**Hidden**는 true일 때, 생성한 작업 스케줄이 사용자 GUI 환경에서 보이지 않도록 합니다. 이는 **보기** 탭에서 **숨겨진 작업 보기**를 활성화 해야 나타납니다.

**ExecutionTimeLimit**은 기록된 시간 이상 작업이 실행되면 중지한다는 의미입니다. 위 그림에서는 P3D로 3Days, 즉 3일을 의미합니다.

-------

- **\<Actions\>**

![actions1.png](/task/actions1.png)

![동작_속성.png](/task/동작_속성.png)

작업 스케줄의 동작에 해당하는 내용입니다.

**Command**는 지정한 프로그램이나 스크립트를 기록합니다.
**Arguments**는 파라미터가 추가될 시 기록되는 필드입니다.
cmd나 powershell을 command로 지정할 시 argument에 명령어를 입력할 수 있습니다.
**WorkingDirectory**는 시작 위치를 지정하는 옵션입니다. 해당 작업 스케줄에 의해 수행되는 내용이 해당 위치에 기록됩니다.

그림에는 test로 적혀있지만, 경로명을 잘 기입해주어야 합니다.
Default 경로는 %Windir%\System32\ 입니다.

-------

- **\<Principals\>**

![principals.png](/task/principals.png)

![docdoc_일반.png](/task/docdoc_일반.png)

작업 스케줄의 일반에서 보안 옵션에 해당되는 내용입니다.

**RunLevel**은 권한을 의미합니다.
위 그림에서 **가장 높은 수준의 권한으로 실행** 옵션을 선택하면 **HighestPrivilege**로 기록되고, 선택하지 않는다면 **LeastPrivilege**로 기록됩니다.

**UserID**는 작업을 실행할 때 사용할 사용자 계정을 의미합니다.

**LogonType**은 **사용자가 로그온 할때만 실행 옵션**을 선택하면 **"InteractiveToken"** 으로 설정됩니다. 이때 사용자는 이미 로그온 되어 있어야 하고 작업은 기존 대화형 세션에서만 실행됩니다.
**로그온 여부에 관계 없이 실행 옵션**을 선택하고 **암호를 저장하지 않는다면 "S4U(Service for User)"** 로 기록됩니다. 이는 시스템에 암호가 저장되지 않으며 네트워크나 암호화된 파일에 액세스 할 수 없습니다.
반면, 암호를 저장한다면 **\"password\"** 로 기록되고, 사용자는 암호를 사용하여 로그온 해야합니다. 


### Powershell

powershell에서는 기존에 CMD에서 사용하던 명령어에서 추가적으로 사용할 수 있는 명령어들이 존재합니다.

주요 ScheduledTask 명령어에 대해 알아보겠습니다.

|Command|Information|
|-|-|
|New-ScheduledTask|예약된 작업 인스턴스를 만듭니다.|
|Get-ScheduledTask|로컬 컴퓨터에 등록된 예약된 작업을 가져옵니다.|
|Register-ScheduledTask|로컬 컴퓨터에 예약된 작업을 등록합니다.|
|Unregister-ScheduledTask|예약된 작업을 등록 취소합니다.|

-------

- **작업 등록 예시**


![powershell_스케줄_생성2.png](/task/powershell_스케줄_생성2.png)

powershell에서 작업 스케줄의 동작과 트리거, 보안 등 세부사항을 등록하기 위해서는 **New-ScheduledTask{변수}** 라고 할 때, 괄호 안의 변수명에 Action, Trigger, Principal 등을 입력하고 그 뒤에 파라미터를 추가하여 입력하여 선언한 변수에 저장합니다.

그리고 $task 변수를 생성해서 New-ScheduledTask 명령어를 입력한 뒤, 뒤에 선언했던 변수들을 추가해줍니다.

마지막으로 **Register-ScheduledTask** 를 통해 작업 스케줄 이름과 인풋에 들어갈 작업 객체를 입력해줍니다.

------ 

- **작업 등록 예시2**

![powershell_스케줄_생성4.png](/task/powershell_스케줄_생성4.png)

위의 예시는 powershell에 인자를 추가하여 실행되도록 하는 작업 스케줄입니다. 
**-w hidden** 명령어는 WindowsStyle Hidden이라는 뜻으로 파워쉘 스크립트 실행 시 파워쉘 콘솔이 실행되지 않도록 합니다.
**-enc** 명령어는 EncodedCommand로 인코딩 된 값을 복호화합니다. 

따라서, 해당 작업 스케줄은 base64로 인코딩 시킨 명령어를 복호화하여 콘솔 실행없이 powershell script 작업이 수행되도록 합니다. 

악의적인 사용자는 로컬 PC의 권한을 탈취해서 몰래 powershell을 통해 작업을 등록해서 은밀하게 수행되도록 할 수 있습니다.

위에서 언급한 -w hidden, -enc 외에도, 공격자가 자주 사용하는 파워쉘 옵션이 더 존재합니다.

파워쉘 스크립트 실행 시 사용자의 프로필을 실행하지 않는 **-nop(NoProfile)**, 파워쉘 프롬프트로 사용자의 입력을 받지 않는 **-noni(NonInteractive)**, 실행 정책 값을 설정하는 **-ep(ExecutionPolicy)** 등이 있습니다.

----- 

- **의심스러운 작업 확인**

CLI 명령어를 활용하여 의심스러운 작업 스케줄을 확인할 수 있습니다.

**schtasks /query /fo LIST /v** 명령어를 통해 전체 작업 스케줄을 자세히 확인할 수 있습니다. findstr이나 select-string 명령어를 concat해서 더 세세하게 검색할 수 있습니다.

- **Example**

-> **schtasks /query /fo LIST /v | findstr "docdoc"**
-> **schtasks /query /fo LIST /v | select-string "exe" -Context 2,3**

![powershel_context.png](/task/powershel_context.png)
Context는 출력에서 > 를 기준으로 첫번째 인수는 이전 출력 라인 수, 두번째 인수는 이후 출력 라인 수를 의미합니다.

----------


gci(Get-ChildItem) 명령어로 하위 경로에 존재하는 파일들을 탐색하여 검색할 수도 있습니다.  

- **Example**

-> **gci -path C:\windows\system32\Tasks -recurse | Select-String Command | FL FileName, Line**

![powershell_검색.png](/task/powershell_검색.png)

-path 로 경로를 설정하고 -recurse 를 통해 하위 모든 항목을 검색합니다. (-depth를 통해 재귀 수준을 제한할 수 있습니다.) 
FL은 FormatList를 의미합니다.

- **Example**

-> **gci -path C:\windows\system32\Tasks -recurse | Select-String 'Command' | ? {$_.FileName -match "docdoc"} | FL FileName, Line**

![powershell_filename_math.png](/task/powershell_filename_math.png)

? 명령을 통해서 키워드 매칭 후 출력도 가능합니다.

------- 

- **이벤트로그를 통해 작업 스케줄러로 실행된 스크립트 확인**

![powershell_난독화_로그_제작.png](/task/powershell_난독화_로그_제작.png)

위 그림은 base64로 인코딩 되어 실행되는 스크립트 예시문입니다. 

![image.png](/task/image.png)

그룹 정책에서 Windows Powershell에 대한 스크립트 블록 로깅이 활성화 되어 있다면, 위의 그림에서 볼 수 있듯이 제작을 위해 사용된 작업 스케줄 명령어(Action)를 확인할 수 있습니다. 

![eventlog_script_log.png](/task/eventlog_script_log.png)

또한, 등록된 작업 스케줄이 실행되었을 때, 실행된 파워쉘 스크립트 작업에 대한 내용을 위의 그림에서 확인할 수 있습니다. 이때, 복호화 된 이벤트 로그는 powershell 버전 5 이상에서 기록됩니다.

![스케줄_등록.png](/task/스케줄_등록.png)

그룹 정책에서 Windows Powershell에 대한 스크립트 블록 로깅이 활성화 되어 있다면, 위 두 그림에서 작업 스케줄을 통해 실행된 파워쉘 스크립트 작업에 대한 내용을 확인할 수 있습니다. 이를 통해 분석 대상 PC에 파워쉘 이벤트 로그 혹은 작업 스케줄 이벤트 로그가 남아 있다면 분석 대상이 될 수 있습니다.

## 마무리

- 분석 대상 이미지에서 의심스러운 작업 스케줄 파일이 남아있다면, xml 파일로 분석하는 것도 좋지만 작업 스케줄러에 등록해서 손쉽게 살펴볼 수 있습니다.

- 악의적인 사용자 및 공격자가 등록해놓은 파워쉘 스크립트를 라이브 환경이라면 쿼리문을 통해서, 이미지 분석 시에는 작업 스케줄 파일이나 파워쉘 이벤트 로그를 통해 살펴볼 수 있습니다. 

## 추가 관련 참조

Powershell : [powershell](/ko/Artifact/Powershell)
이벤트로그 : [이벤트로그](/ko/Artifact/Evtx)