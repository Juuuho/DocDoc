---
title: Advanced
description: 
published: true
date: 2022-11-25T09:56:51.950Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:57:23.723Z
---

# ShellBag

- 분석 목적 : ShellBag을 통해 파악할 수 있는 정보를 확인하고, 관련 툴을 알아봅니다.

- 분석 환경 : Microsoft Windows 10 Pro

- Analysis Files : 
|Analysis Files|Artifacts|
|-|-|
|HKEY_CURRENT_USER\Software\Classes\Local Settings\Software\Microsoft\Windows\Shell\Bags|Bags|
|HKEY_CURRENT_USER\Software\Classes\Local Settings\Software\Microsoft\Windows\Shell\BagMRU|BagMRU|
|HKEY_CURRENT_USER\Microsoft\Windows\Shell\Bags|Bags|



- 분석 도구

|도구|버전|URL|
|-|-|-|
|ShellBagsExplorer.exe|1.4.0.0|https://www.sans.org/tools/shellbags-explorer/|
|레지스트리편집기|-|-|


## 상세분석
Registry 관련 ShellBag은 Bags와 BagMRU 두 종류가 존재합니다. ShellBag은 최초로 폴더를 열람 시 생성되며, 폴더를 생성하거나 복사하는 경우에도 생성되긴 하나, 그렇지 않은 경우도 있습니다.

중요한 것은 BagMRU에 존재하는 MRUListEx를 통해 최근에 접근한 폴더의 순서 정보를 확인할 수 있다는 것입니다. 분석에 앞서 알고 진행해야 될 부분은 다음과 같습니다.

1. BagMRU key 시간에는 최초로 생성될 때의 시간이 설정되며, 최초 생성 = 최초 폴더 열람
2. Bags key 시간이 바뀌는 경우는 폴더 설정 변경 시
3. key 시간이 갱신되는 경우가 존재하며, 현재 폴더 생성 및 열람에 의해 상위 폴더의 key 시간이 갱신

## Bags
Bags 레지스트리 키는 창의 크기, 위치 및 보기 모드 같은 기본 설정을 저장하고 있습니다.

### HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\Shell\Bags\1\Desktop
![bags2.png](/shellbag/bags2.png)
경로에서 확인할 수 있듯이 Desktop 관련 폴더의 설정 정보를 가지고 있는 Bags 구조와 값입니다.

### HKEY_CURRENT_USER\SOFTWARE\Classes\Local Settings\Software\Microsoft\Windows\Shell\Bags
![bags1.png](/shellbag/bags1.png)
"HKEY_CURRENT_USER\SOFTWARE\Classes\Local Settings\Software\Microsoft\Windows\Shell\Bags" 경로에 존재하는 폴더를 확인하면 위 사진처럼 해당 폴더의 설정 값을 확인할 수 있습니다.

## BagMRU
BagMRU는 분석 대상 PC의 사용자가 접근한 폴더의 순서를 확인할 수 있습니다. 해당 키에 존재하는 값과 설명은 다음과 같습니다.

|키 값|설명|
|-|-|
|MRUListEx|사용자가 접근한 폴더의 순서|
|NodeSlot|Bags의 하위키 이름|
|NodeSlots|2의 고정 값|


### HKEY_CURRENT_USER\Software\Classes\Local Settings\Software\Microsoft\Windows\Shell\BagMRU
![mrust.png](/shellbag/mrust.png)
BagMRU 구조는 위 사진과 같습니다. BagMRU 키의 값들을 확인하면 앞서 설명한 MRUListEx, NodeSlot, NodeSlots가 존재하는 것을 확인할 수 있습니다. 또한 MRUListEx에 존재하는 값의 16진수 번호들의 값들 또한 MRUListEx 위(키 내부에서 위치상)에 존재합니다.

먼저 폴더의 접근 순서를 가지고 있는 MRUListEx를 분석합니다.

#### MRUListEx
![mrulist.png](/shellbag/mrulist.png)
MRUListEx를 확인하면 위의 사진처럼 95라는 16진수 값이 가장 앞을 차지하고 있는 것을 확인할 수 있습니다. 해당 값은 폴더의 번호이며 가장 최근에 접근한 폴더입니다. 95(hex)는 149(dec)입니다. 가장 최근에 접근한 폴더의 번호를 식별했다면 번호를 기억하고 해당 폴더 번호의 값을 찾습니다.

#### 149(dec)번호 폴더 확인
![mru3.png](/shellbag/mru3.png)
"값 이름(N):"에서 확인할 수 있듯이 149번 폴더를 의미하는 키 값 정보입니다. 오른쪽 String 정보에서 "Datg"로 밑줄되어 있는 부분은 폴더 이름을 의미합니다. ShellBagMRU Structure는 다음 단락에서 자세히 분석하겠습니다.

#### ShellBagMRU 구조
![mru2.png](/shellbag/mru2.png)
ShellBagMRU 구조는 크게 Block 구조와 Extension Block 구조를 가지고 있습니다. 먼저, Block 구조는 다음 표와 같습니다.


|주소|크기(bytes)|파일 이름|요약|
|-|-|-|-|
|0x00~0x01|2|Block Size|Block Size|
|0x02~0x03|2|Type|0x31=폴더,0x32=파일|
|0x04~0x07|4|File Size|File Size|
|0x08~0x0B|4|Modification Time|Modification Time|
|0x0C~0x0D|2|Type|0x10:폴더, 0x20 Zip|
|0x0E~NameSize|?|Short Name|폴더 이름에 따라 길이가 다름|

Extension Block 구조는 다음과 같습니다.

|주소|크기(bytes)|파일 이름|요약|
|-|-|-|-|
|0x00~0x01|2|Extension Block Size|Extension Block Size|
|0x02~0x03|2|Version|Version|
|0x04~0x07|4|Signature|0xBEEF0004|
|0x08~0x0B|4|Create Time|Create Time|
|0x0C~0x0F|4|Last Access Time|Last Access Time|
|0x10~0x13|4|Identifier|0x2E:Windows8.1, Windows10|
|0x14~0x17|4|MFT Entry Number|MFT Entry Number|
|0x18~0x19|2|Reserved Area|set 0x00|
|0x1A~0x1B|2|MFT Sequence Number|MFT Sequence Number|
|0x1C~0x29|14|Reserved Area|set 0x00|
|0x2A~0x2D|4|CheckSum|folder unique number|
|0x2E~0x??|?|File Name|폴더 이름에 따라 길이가 다름|
|0x(마지막-4)|4|Extension Block Offset|Starting point Extension Block|

위의 구조를 통해 분석을 진행한다면 폴더의 접근 순서, MAC Time 등을 파악할 수 있습니다.

## ShellBags Explorer 도구 사용
![sbex.png](/shellbag/sbex.png)

ShellBags 분석에는 SANS에서 제공하는 ShellBags Explorer 도구를 사용해 분석할 수 있습니다.

위 사진에서 좌상단에 있는 [File]을 통해 "active Registry" 또는 "offline hive"를 불러옵니다. "active Registry"는 현재 프로그램을 작동시키는 PC가 될 수 있으며, "offline hive"는 분석 대상 PC에서 뽑아온 hive 파일을 로드할 수 있습니다.

Value 인터페이스에 제공되는 폴더들은 접근 순서대로 나열되어 있으며, 해당 폴더를 선택하면 Summary, Details, Hex로 해당 폴더의 정보를 확인할 수 있습니다. 직접 레지스트리를 분석하는 것도 중요하지만 해당 도구를 통해서 쉽게 파악할 수 있습니다.

## ShellBags 활용
레지스트리 Shellbags은 "탐색기 프로그램(explorer.exe)"이나 "GetOpenFileNameAPI"를 통해 폴더를 열람/이동 할 때 마다 실시간으로 갱신됩니다. 해당 기능을 통해 열람/이동이 되지 않고, CMD 명령어(cd, cat 등)로 접근하는 경우 레지스트리 변환는 없습니다.

따라서 CMD, PowerShell을 통해서 악성코드를 다운 받는 행위나 실행시키는 행위를 가지는 침해 사고, 파일 유출에는 의미를 가지기 힘듭니다. 그러나 사용자가 직접적(위에 설명한 조건)으로 파일을 유출하거나, 악성행위를 진행한다면 중요한 의미를 가질 수 있습니다.

레지스트리의 Shellbags 정보 갱신을 실험하고 분석한 사이트는 "추가 관련 참조"에서 확인할 수 있습니다.

## 마무리
- ShellBag에 관련 분석 방법과 도구를 확인할 수 있었습니다. PC 사용자의 폴더 접근 기록을 확인할 수 있기 때문에 특정 상황에 유용한 아티팩트로 사용될 수 있습니다.

## 추가 관련 참조
- Registry: [Registry](/ko/Artifact/Registry)
- Shellbags 생성/갱신 기준 - geun-yeong: https://geun-yeong.tistory.com/5
- Shellbags(Windows10) - c0wb3ll: https://c0wb3ll.tistory.com/entry/ShellBags-Windows10#--%--Windows--%--ShellbagMRU%--Structure%---%--Block%--Struture