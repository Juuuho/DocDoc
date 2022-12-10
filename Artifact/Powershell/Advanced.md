---
title: Advanced
description: Powershell_Advanced
published: true
date: 2022-12-03T12:23:59.270Z
tags: 
editor: markdown
dateCreated: 2022-11-26T02:32:20.202Z
---

# Powershell

본 페이지는 Powershell에 대해 더 자세히 분석한 내용을 작성하였습니다.

- 분석 환경 : Windows 10 21H2


## 상세 분석

### Execution Policy
Execution Policy는 파워쉘 실행정책을 의미합니다. Execution Policy는 파워쉘이 가진 첫번째 보안 방식 중 하나이며, 파워쉘이 실행할 스크립트 제어와 관련된 설정으로 이해하시면 됩니다.

![ps1.png](/ps_ad/ps1.png)
보통 파워쉘을 실행하게 되면 외부에서 가져온 스크립트를 실행할 때
"이 시스템에서 스크립트를 실행할 수 없으므로~"라는 말과 함께 맨 하단부에 **UnauthorizedAccess** 라는 에러 메시지가 출력됩니다.

해당 에러 메시지는 파워쉘에서 특정 ps1 파일을 실행할 때 실행 정책에 의해 실행이 제한된 것을 의미합니다.


![ps2.png](/ps_ad/ps2.png)
실행 정책은 위 그림과 같이 **Get-ExecutionPolicy -List** 명령을 통해 확인할 수 있습니다.

Scope에 따라 ExecutionPolicy가 달라질 수 있습니다.

Scope에는 다음 표와 같이 5가지 종류가 존재합니다.

- **Scope**

|Scope|정보|저장경로|
|-|-|-|
|MachinePolicy|GPO의 컴퓨터 정책을 통해 설정하는 경우|HKLM:\Software\Policies\Microsoft\Windows\Powershell|
|UserPolicy|GPO의 사용자 정책을 통해 설정하는 경우|HKCU:\Software\Policies\Microsoft\Windows\Powershell|
|Process|현재 실행 중인 파워쉘 세션에만 적용되는 실행정책|-|
|CurrentUser|현재 사용자에 적용되는 실행정책|HKCU:\Software\Microsoft\Powershell\1\ShellIds|
|LocalMachine|로컬 컴퓨터에 적용되는 실행정책|HKLM:\Software\Microsoft\Powershell\1\ShellIds|

위 저장경로에 존재하지 않는다면 해당 Scope에 대한 ExecutionPolicy는 Undefined로 설정됩니다.


- **ExecutionPolicy**

|ExecutionPolicy|정보|
|-|-|
|Undefined|ExecutionPolicy를 설정하지 않았다는 의미이며, 기본 정책인 "Restricted"로 작동합니다.|
|Restricted|Win 10의 기본 값이며, 이 경우 ps 스크립트 파일이 실행되지 않습니다. 다만, MS에서 만든 일부 서명된 스크립트 파일은 실행이 가능하기도 합니다.|
|Unrestricted|MS에서도 권장하지 않는 옵션이며, 모든 스크립트를 실행할 수 있습니다.|
|AllSigned|신뢰할 수 있는 인증기관이 서명한 스크립트만 실행하는 옵션, 해당 컴퓨터에서 작성된 스크립트라 하더라도 신뢰할 수 있는 인증기관이 서명하지 않았다면 실행이 불가능합니다.
|ByPass|다른 어플리케이션 내에 파워쉘 스크립트가 내장되거나, 별도의 자체 보안 설정을 갖추었을 때 사용하기 위해 만들어졌습니다. 차단되거나 별다른 경고 없이 실행됩니다.|
|RemoteSigned|최신 Windows Server 버전의 Powershell 실행정책 기본값입니다. 해당 로컬 컴퓨터에서 작성된 모든 스크립트는 실행이 가능하며, 인터넷에서 다운로드한 스크립트는 인증기관이 발행한 코드로 서명되어야만 실행 가능합니다.|


따라서, ps 스크립트를 로컬 내에서 더블클릭으로 실행하거나 powershell을 통해 실행하기 위해선 ExecutionPolicy를 변경해주어야 합니다.

변경은 **Set-ExecutionPolicy** 명령어로 가능합니다.

기본적으로 **Set-ExecutionPolicy RemoteSigned**로 실행하게 되면 LocalMachine scope에 적용됩니다. 

![ps3.png](/ps_ad/ps3.png)

위 그림에서는 -scope를 지정해서 ExecutionPolicy를 변경할 수도 있으며, -force 옵션과 함께 사용할 경우 "변경하시겠습니까?"라는 이벤트 없이 바로 적용됩니다.

![ps4.png](/ps_ad/ps4.png)

powershell.exe를 통해 ExecutionPolicy를 변경하는 경우 해당 실행 세션에서만 변경 여부가 적용됩니다.

### Script Logging

![ps5.png](/ps_ad/ps5.png)

위의 Execution Policy를 실행하면서도 스크립트 블록 로깅을 활성화해놓는다면 이벤트 로그에 흔적이 남습니다.

Windows 10에서는 PSReadLine폴더 내 ConsoleHost_history.txt 파일에 파워쉘 명령어 로그가 저장됩니다. 해당 명령은 파워쉘 콘솔창 내에서 다이렉트로 명령어를 실행하는 경우 실행됩니다. 

또한, base64로 인코딩 된 명령 역시 다이렉트로 명령어를 실행했을 때 기록되는 것을 알 수 있습니다.

예시를 통해 살펴보도록 하겠습니다.

- **Example**

![ps6.png](/ps_ad/ps6.png)

파일 다운로드 명령과 이에 대한 base64 인코딩 명령을 실행합니다.


![ps7.png](/ps_ad/ps7.png)

위 그림의 ConsoleHost_History.txt 파일에서 두 명령 모두 기록되는 것을 확인할 수 있습니다.

하지만, ConsoleHost_History.txt에서는 인코딩 된 스크립트에 대한 복호화 된 스크립트가 기록되지 않지만, 이벤트로그에서는 기록되는 것을 확인하실 수 있습니다.


![ps9.png](/ps_ad/ps9.png)

base64로 인코딩 된 파워쉘 스크립트 로깅 기록입니다. 오후 9시 11분 26초에 실행되었으며, 

![ps8.png](/ps_ad/ps8.png)

곧바로, 복호화된 스크립트 로그가 기록되는 것을 확인하실 수 있습니다.

자세한 로깅 여부 기록은 다음의 표를 통해 확인할 수 있습니다.

![ps10.png](/ps_ad/ps10.png)
**출처 : Plainbit**

## 마무리
- powershell script 실행 흔적을 분석하기 위해서 공격자가 ExecutionPolicy를 변경할 수도 있기 때문에 해당 내용을 아는 것이 도움이 될 수 있습니다.
- base64로 인코딩 된 스크립트에 대한 복호화 된 스크립트는 이벤트로그에서 확인 가능합니다. 

## 추가 관련 참조

- ExecutionPolicy : https://m.blog.naver.com/vanstraat/221732533202

- Windows 10 Script Logging 여부 : http://blog.plainbit.co.kr/archives/3789

