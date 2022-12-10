---
title: Windows Defender 공격 행위
description: Windows Defender 공격 행위 분석
published: true
date: 2022-11-28T09:48:29.859Z
tags: 
editor: markdown
dateCreated: 2022-11-28T07:02:02.416Z
---

# Windows Defender 공격 행위

- 분석 목적: 악성코드가 분석 대상 PC에 침투하여 Windows Defender를 공격했을 때 확인할 수 있는 흔적을 찾고 분석합니다.

- 분석 환경 : Microsoft Windows 10 Pro

- Analysis Files : 

|Analysis Files|Artifacts|
|-|-|
|Microsoft-Windows-PowerShell/Operational.evtx|이벤트 로그|
|HKLM\SOFTWARE\Policies\Microsoft\Windows Defender\MpEngine|Registry|
|HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection|Registry|
|HKLM\Software\Policies\Microsoft\Windows Defender\Reporting|Registry|
|HKLM\Software\Policies\Microsoft\Windows Defender\SpyNet|Registry|
|Microsoft\Windows\Windows Defender\Windows Defender Cache Maintenance|File|
|Microsoft\Windows\Windows Defender\Windows Defender Cleanup|File|
|Microsoft\Windows\Windows Defender\Windows Defender Verification|File|
|Microsoft\Windows\ExploitGuard\ExploitGuard MDM policy Refresh|File|
|MpCmdRun.log|Log File|

- 분석 도구

|도구|버전|URL|
|-|-|-|
|이벤트 뷰어|-|-|
|REGA|v1.5.3.0|http://forensic.korea.ac.kr/tools.html|
|Visual Studio Code|Version 1.73|https://code.visualstudio.com/|

## 이벤트 로그 중점

이벤트 로그 중점으로 확인하는 MS 보안 시스템인 Windows Defender를 공격한 행위입니다. 실제 랜섬웨어의 행위를 예시로 확인하며, 학습을 진행합니다.


![windowsdefender_ioavprotection.png](/windowsdefender/windowsdefender_ioavprotection.png)

위 그림에서 확인할 수 있는 IOAVProtection 비활성화 명령어는 실제로 랜섬웨어가 수행한 명령어입니다. IOAVProtection은 Windows Defender가 다운로드한 모든 파일과 첨부 파일을 검색하는 보호기법 중 하나입니다. 랜섬웨어가 실제로 PowerShell 명령어를 이용해 Windows Defender 관련 보호기법을 제거하는 것을 확인할 수 있습니다.

![windowsdefender_realtime.png](/windowsdefender/windowsdefender_realtime.png)

위 그림은 Windows Defender 실시간 감지를 비활성화하는 PowerShell 로그입니다. 명령어 내용 중 "- DisableRealtimeMonitoring $true"를 이용해 실시간 감지를 비활성화 하는 것을 확인할 수 있습니다(Windows Defender는 사용자가 끄지 않는 한 지속적으로 탐지를 진행합니다).

## Registry 중점

Registry 중점으로 확인하는 Windows Defender 관련 공격 흔적입니다.

### MpEngine
![windowsdefender_registry1.png](/windowsdefender/windowsdefender_registry1.png)

|값|명령어 예시|
|-|-|
|MpEnablePus|reg.exe add "HKLM\Software\Policies\Microsoft\Windows Defender\MpEngine" /v "MpEnablePus" /t REG_DWORD /d "0" /f|


위 그림은 "HKLM\SOFTWARE\Policies\Microsoft\Windows Defender\MpEngine" 경로에서 확인할 수 있습니다. MpEnablePus는 Windows Defender의 하위 키인 MpEngine에 존재하는 값이며, Windows Defender의 팝업 경고를 활성/비활성화할 수 있는 값을 저장하고 있습니다. 해당 값이 1이면 활성화, 0이면 비활성화 입니다. 기본적으로 1로 활성화 되어 있습니다. 만약 위 표와 같은 명령어가 실행된다면 위의 그림과 같이 0으로 비활성화됩니다.


### Real-Time Protection
![windowsdefender_registry2.png](/windowsdefender/windowsdefender_registry2.png)

|값|명령어 예시|
|-|-|
|DisableIOAVProtection|reg.exe add "HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection" /v "DisableIOAVProtection" /t REG_DWORD /d "1" /f|
|DisableBehaviorMonitoring|reg.exe add "HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection" /v "DisableBehaviorMonitoring" /t REG_DWORD /d "1" /f|
|DisableOnAccessProtection|reg.exe add "HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection" /v "DisableOnAccessProtection" /t REG_DWORD /d "1" /f|
|DisableRealtimeMonitoring|reg.exe add "HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection" /v "DisableRealtimeMonitoring" /t REG_DWORD /d "1" /f|
|DisableScanOnRealtimeEnable|reg.exe add "HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection" /v "DisableScanOnRealtimeEnable" /t REG_DWORD /d "1" /f|

위 그림은 "“HKLM\SOFTWARE\Policies\Microsoft\Windows Defender\Real-Time Protection"에서 확인할 수 있습니다. Real-Time Protection 키는 실시간 보호 활성/비활성화와 관련된 키 값들을 가지고 있습니다. 만약 위 표와 같은 명령어가 실행된다면 위 사진의 값들은 모두 1로 설정됩니다. 해당 키 값들은 Disable이 1(True)로 활성화되기 때문에 비활성화됩니다.

### Reporting

![windowsdefender_registry3.png](/windowsdefender/windowsdefender_registry3.png)

|값|명령어 예시|
|-|-|
|DisableEnhancedNotifications|reg.exe add "HKLM\Software\Policies\Microsoft\Windows Defender\Reporting" /v "DisableEnhancedNotifications" /t REG_DWORD /d "1" /f|

위 그림은 “HKLM\SOFTWARE\Policies\Microsoft\Windows Defender\Reporting"에서 확인할 수 있습니다. Windows Defender 관련 Report 기능을 활성/비활성화할 수 있는 키 값을 가지고 있습니다. 만약 위 표와 같은 명령어가 실행된다면 위 DisableEnhanceNotifications 값을 1로 설정하여 Reporting 기능이 비활성화 됩니다. Default는 0으로 활성화되어 있습니다.


### SpyNet

![windowsdefender_registry4.png](/windowsdefender/windowsdefender_registry4.png)

|값|명령어 예시|
|-|-|
|DisableBlockAtFirstSeen|reg.exe add "HKLM\Software\Policies\Microsoft\Windows Defender\SpyNet" /v "DisableBlockAtFirstSeen" /t REG_DWORD /d "1" /f|
|SpynetReporting|reg.exe add "HKLM\Software\Policies\Microsoft\Windows Defender\SpyNet" /v "SpynetReporting" /t REG_DWORD /d "0" /f|
|SubmitSamplesConsent|reg.exe add "HKLM\Software\Policies\Microsoft\Windows Defender\SpyNet" /v "SubmitSamplesConsent" /t REG_DWORD /d "0" /f|

위 그림은 "“HKLM\Software\Policies\Microsoft\Windows Defender\SpyNet"에서 확인할 수 있습니다. Windows Defender의 기능 중 하나인 잠재적 스파이웨어 탐지 및 차단 기능을 수행하는 값들이 저장되어 있습니다. 만약 위 표와 같은 명령어가 실행된다면 위 그림처럼 차례대로 차단, 리포팅, 감지된 샘플 전송 기능이 비활성화 됩니다.

## 작업 스케줄러 중점

작업 스케줄러 중점으로 확인하는 Windows Defender 관련 공격 흔적입니다.

### Windows Defender Cache Maintenance

![windowsdefender_sche1.png](/windowsdefender/windowsdefender_sche1.png)

|값|명령어 예시|
|-|-|
|Windows Defender Cache Maintenance|schtasks.exe /Change /TN "Microsoft\Windows\Windows Defender\Windows Defender Cache Maintenance" /Disable|

"Windows Defender Cache Maintenance"는 Windows Defender에서 격리 중인 임시 파일을 저장하는 공간입니다. 만약 위 표의 명령어가 실행된다면 위 그림처럼 Windows Defender의 캐시 유지를 비활성화 할 수 있습니다.

### Windows Defender Cleanup

![windowsdefender_sche2.png](/windowsdefender/windowsdefender_sche2.png)

|값|명령어 예시|
|-|-|
|Windows Defender Cleanup|schtasks.exe /Change /TN "Microsoft\Windows\Windows Defender\Windows Defender Cleanup" /Disable|

Windows Defender Cleanup은 악성프로그램 감염이 시작됐는지 정기적으로 레지스트리 검사를 실행합니다. 만약 위 표의 명령어가 실행된다면 위 그림처럼 Windows Defender Cleanup 기능이 비활성화 될 수 있습니다.

### Windows Defender Verification

![windowsdefender_sche3.png](/windowsdefender/windowsdefender_sche3.png)

|값|명령어 예시|
|-|-|
|Windows Defender Verification|schtasks.exe /Change /TN "Microsoft\Windows\Windows Defender\Windows Defender Verification" /Disable|

Windows Defender Verification은 Windows Defender의 검증 기능을 수행합니다. 만약 위 표의 명령어가 실행된다면 위 그림처럼 Windows Defender Verification 기능이 비활성화 될 수 있습니다.

### ExploitGuard MDM policy Refresh

![windowsdefender_sche4.png](/windowsdefender/windowsdefender_sche4.png)

|값|명령어 예시|
|-|-|
|ExploitGuard MDM policy Refresh|schtasks.exe /Change /TN "Microsoft\Windows\ExploitGuard\ExploitGuard MDM policy Refresh" /Disable|

ExploitGaurd MDM policy Refresh는 Windows Defender Exploit Guard의 구성 요소 중 하나입니다. Exploit 관련 사항을 보호하는 역할을 수행하며, 만약 위 표의 명령어가 실행된다면 해당 기능이 비활성화 될 수 있습니다.

## Log 파일 중점
프로그램이나 명령어 유틸같은 경우 log 파일이 남는 경우가 있습니다. 따라서 분석하고 싶은 대상의 log 파일을 찾아 분석하는 것도 분석 방법 중 하나입니다. 다음 소단락에서는 Windows Defender 관련 log 파일을 확인합니다.

### MpCmdRun.log

![windowsdefender_log.png](/windowsdefender/windowsdefender_log.png)

|값|명령어 예시|
|-|-|
|MpCmdRun|cmd.exe /c "C:\Program Files\Windows Defender\MpCmdRun.exe" -RemoveDefinitions -All|

위 그림은 MpCmdRun.exe 실행 후 남는 기록인 MpCmdRun.log 파일입니다. MpCmdRun.exe는 Windows Defender용 Malware 방지 명령어 유틸이며, 만약 위 표와 같은 명령어가 실행된다면 MpCmdRun.log 파일에서 실행 기록을 확인 가능합니다.

## 마무리
- 분석 대상 PC에서 Windows Defender를 공격하는 악성행위 흔적의 예시를 살펴봤습니다. 기본적으로 Windows Defender는 정상적인 Windows OS 환경에서 활성화 된 상태로 운영되고 있습니다. 이러한 보안 기법을 차단하거나 비활성화하는 행위는 악성행위로 의심할 수 있으며, 본 페이지에서 설명한 예시와 같은 상황을 수집한다면 침해사고 분석시에 도움이 될 수 있습니다.

## 추가 관련 참조
- Windows Defender Evtx: [WindowsDefender](/ko/Artifact/Evtx/응용프로그램및서비스로그/Advanced/WindowsDefender)
- Registry: [Registry](/ko/Artifact/Registry)
- Windows Defender 관련 이벤트 로그: https://learn.microsoft.com/ko-kr/microsoft-365/security/defender-endpoint/troubleshoot-microsoft-defender-antivirus?view=o365-worldwide