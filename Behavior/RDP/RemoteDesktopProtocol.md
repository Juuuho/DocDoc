---
title: 원격 데스크톱 연결
description: RemoteDesktopProtocol_RDP_Windows 원격
published: true
date: 2022-11-26T05:40:35.796Z
tags: 
editor: markdown
dateCreated: 2022-11-26T05:40:33.092Z
---

# 원격 데스크톱 연결

- 분석 목적 : 분석 대상 PC의 원격 데스크톱 연결 관련 흔적을 분석합니다.

- 분석 환경 : Microsoft Windows 10 Pro

- Analysis Files : 

|Analysis Files|Artifacts|
|-|
|Security.evtx|이벤트 로그|
|Microsoft-Windows-TerminalServices-LocalSessionManager%4Operational.evtx|이벤트 로그|
|HKEY_USERS\{SID}\SOFTWARE\Microsoft\Terminal Server Client\Default|레지스트리|
|HKEY_USERS\{SID}\SOFTWARE\Microsoft\Terminal Server Client\Servers\{IP}|레지스트리|

- 분석 도구

|도구|버전|URL|
|-|-|-|
|이벤트 뷰어|-|-|
|레지스트리 편집기|-|-|


- VM

|OS|버전|System Name|
|-|-|-|
|Microsoft Windows 10 Pro|10.0.19044 Build 19044.1826|DESKTOP-M3J1EH3|

## 상세분석
Windows OS에서 지원하는 "원격 데스크톱 연결(Remote Desktop Services, RDP)"을 사용하면 같거나 다른 네트워크로 접속이 가능합니다. 해당 기능은 보통 원격지에서 업무를 보거나 원격으로 PC의 문제를 제거하는데 사용되기도 합니다. 그러나 악용된다면 침해사고의 경로가 될 수 있습니다. 
본 페이지에서는 분석 대상 PC에서 다른 PC로 연결, 다른 PC에서 분석 대상 PC로 연결 시에 남는 흔적들을 수집 및 분석한 결과를 기술합니다.


## 이벤트 로그
원격 데스크톱 연결 기록은 대부분 이벤트 로그에 저장됩니다. 따라서 본 페이지에서는 evtx 파일을 기준으로 이벤트 로그 분석을 진행합니다.

### RDP 연결 시 이벤트 로그 저장 순서
본격적인 분석 이전에 RDP 연결을 진행 시 기록되는 로그를 시간 순서대로 파악하여, 전체적인 구조를 확인합니다.
|연결 순서|
|-|
|네트워크 연결 ➜ 인증 ➜ 로그온 ➜ 세션 연결 ➜ 세션 연결 해제/재연결 ➜ 로그오프|


### Security.evtx
Security.evtx에는 원격 데스크톱 연결과 관련된 로그가 저장되어 있습니다. 실제 원격 접속 시에 확인할 수 있는 주요 로그를 분석합니다.

#### 이벤트 ID: 4624
> 계정이 성공적으로 로그온되었습니다.
> 
> 주체:
> 	보안 ID:		NULL SID
> 	계정 이름:		-
> 	계정 도메인:		-
> 	로그온 ID:		0x0
> 
> 로그온 정보:
> 	로그온 유형:		3
> 	제한된 관리 모드:	-
> 	가상 계정:		아니요
> 	상승된 토큰:		아니요
> 
> 가장 수준:		가장
> 
> 새 로그온:
> 	보안 ID:		DESKTOP-M3J1EH3\juhoheo
> 	계정 이름:		juhoheo
> 	계정 도메인:		DESKTOP-M3J1EH3
> 	로그온 ID:		0xE7A37F
> 	연결된 로그온 ID:		0x0
> 	네트워크 계정 이름:	-
> 	네트워크 계정 도메인:	-
> 	로그온 GUID:		{00000000-0000-0000-0000-000000000000}
> 
> 프로세스 정보:
> 	프로세스 ID:		0x0
> 	프로세스 이름:		-
> 
> 네트워크 정보:
> 	워크스테이션 이름:	LAPTOP-A1QNQ5MA
> 	원본 네트워크 주소:	192.168.48.1
> 	원본 포트:		0
> 
> 인증 세부 정보:
> 	로그온 프로세스:		NtLmSsp 
> 	인증 패키지:	NTLM
> 	전송된 서비스:	-
> 	패키지 이름(NTLM 전용):	NTLM V2
> 	키 길이:		128

ID: 4624 이벤트 로그는 계정이 성공적으로 로그인되었는지 기록합니다. 위의 로그기록에서 "로그온 유형: 3"은 네트워크 연결을 뜻하며 RDP 연결 시에 3으로 기록됩니다. 새 로그온에는 원격 요청을 받은 PC의 정보가 기록됩니다. 또한 네트워크 정보에는 원격을 요청한 PC의 이름과 네트워크 주소가 기록됩니다.
해당 로그는 침해사고 분석시에 원격으로 공격을 감행한 PC의 IP와 언제 원격을 시도했는지를 파악할 수 있는 중요한 이벤트 로그입니다.


|이벤트 ID|설명|
|-|-|
|4624|계정이 성공적으로 로그인 되었습니다.|
|4625|계정에 로그온하지 못했습니다.|
|4634|계정이 로그오프되었습니다.|
|4647|사용자가 시작한 로그오프:|
|4778|세션이 Window Station에 다시 연결되었습니다.|
|4779|세션이 Window Station에서 연결이 끊겼습니다.|

위의 표는 RDP 연결과 관련된 Security로그입니다.

### Microsoft-Windows-TerminalServices-LocalSessionManager%4Operational.evtx
![이벤트25.png](/원격데스크톱연결/이벤트25.png)

ID: 25는 "Microsoft-Windows-TerminalServices-LocalSessionManager%4Operational.evtx"에서 확인할 수 있는 원격 관련 이벤트 로그입니다. 요청 받은 PC의 계정 정보와 요청한 PC의 네트워크 주소, 접속 시각을 유추할 수 있는 정보가 기록되어 있습니다.

|Event ID|설명|
|-|-|
|40|1 세션의 연결이 끊어졌습니다. 이유 코드 5|
|24|원격 데스크톱 서비스: 세션 연결 끊김:|
|25|원격 데스크톱 서비스: 세션 다시 연결 성공:|

위 표는 로컬 호스트가 접속되어 있는 상태에서 원격 데스크톱 연결을 수행한 후에 생기는 로그 정보입니다. 위에서부터 아래로 기록됩니다.

### 원격 관련 이벤트 로그

이전에 설명했던 주요한 이벤트 로그를 포함하여 원격 관련 이벤트 로그를 표로 나타낸 결과입니다.

|이벤트 뷰어 경로|Event ID|설명|
|-|-|
|Security|4624|계정이 성공적으로 로그온되었습니다. - 로그온 유형: 3은 네트워크 접속을 의미|
|Security|4648|명시 적 자격 증명을 사용하여 로그온을 시도했습니다.|
|Security|1024|RDP ClientActiveX가 서버(IP.ADDRESS 또는 HOSTNAME)에 연결을 시도하고 있습니다. - RDP 클라이언트 MSTSC.exe를 사용하여 RDP 연결을 시도하면 생성|
|TerminalServices-ClientActiveXCore|1025|RDP ClientActiveX가 서버에 연결되었습니다.|
|TerminalServices-ClientActiveXCore|1027|세션 4로 도메인(RDS-MS)에 연결되었습니다.|
|TerminalServices-ClientActiveXCore|1028|서버가 SSL을 지원함 = 지원됨|
|TerminalServices-ClientActiveXCore|1029|Base64(SHA256(UserName))|
|TerminalServices-ClientActiveXCore|1102|클라이언트가 서버 IP에 대한 |
|TerminalServices-RemoteConnectionManager|261|리스너 RDP-Tcp가 연결을 수신했습니다. - RDP 로그인을 시도하면 성공하든 실패하든 기록에 남음|
|TerminalServices-RemoteConnectionManager|1149|원격 데스크톱 서비스: 사용자 인증 성공|
|TerminalServices-LocalSessionManager|24|원격 데스크톱 서비스: 세션 연결이 끊어졌습니다.|
|TerminalServices-LocalSessionManager|25|원격 데스크톱 서비스: 세션 다시 연결 성공|
|TerminalServices-LocalSessionManager|40|세션 연결 끊김|
|TerminalServices-LocalSessionManager|41|세션 중재 시작|
|TerminalServices-LocalSessionManager|42|세션 중재 종료|

## 레지스트리
원격을 요청한 PC에서 확인할 수 있는 원격 관련 레지스트리 정보입니다.

### HKEY_USERS\{SID}\SOFTWARE\Microsoft\Terminal Server Client\Default
![regsitry_terminalserverclient.png](/원격데스크톱연결/regsitry_terminalserverclient.png)

위에서 확인할 수 있듯이 RDP를 이용한 연결 대상 IP를 확인할 수 있습니다. MRU9까지 기록되며, MRU0이 가장 마지막으로 연결한 PC의 IP를 나타냅니다.

### HKEY_USERS\{SID}\SOFTWARE\Microsoft\Terminal Server Client\Servers\{IP}
![regsitry_terminalserverclient2.png](/원격데스크톱연결/regsitry_terminalserverclient2.png)

또한 위 경로에서는 연결한 PC의 IP의 도메인 또는 계정명이 저장됩니다.


## 마무리
원격 데스크톱 연결(RDP) 관련 흔적을 이벤트 로그, 레지스트리를 중점으로 분석했습니다. 원격 관련 흔적 중에서는 이벤트 로그에 원격 접속 방식에 따른 기록이 저장되며, 다양한 곳에 저장됩니다. 또한 원격 접속을 요청한 PC에서 레지스트리 분석을 통해 가장 마지막에 접근한 PC부터 가장 최근에 접근한 PC까지의 IP 정보를 확인할 수 있고, 연결했던 계정을 파악할 수 있습니다.


## 추가 관련 참조
- Evtx: [Evtx](/ko/Artifact/Evtx)
- Registry: [Registry](/ko/Artifact/Registry)
- 논문 - 이벤트 로그 분석: https://scienceon.kisti.re.kr/srch/selectPORSrchArticle.do?cn=DIKO0015070499&dbt=DIKO