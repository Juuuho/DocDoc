---
title: Advanced
description: Windows.edb_Advanced
published: true
date: 2022-11-25T04:14:11.553Z
tags: 
editor: markdown
dateCreated: 2022-11-22T07:04:41.786Z
---

# Windows.edb

- 분석 목적 : Windows.edb를 분석할 수 있는 도구인 WinSearchDBAnalyzer 통해 확인 가능한 정보를 분석합니다.

- 분석 환경 : Microsoft Windows 10 Pro

- Analysis Files : 

|Analysis Files|Artifacts|
|-|-|
|C:\ProgramData\Microsoft\Search\Data\Applications\Windows\Windows.edb|Windows.edb|

- 분석 도구

|도구|버전|URL|
|-|-|-|
|WinSearchDBAnalyzer|1.0.0.6|http://moaistory.blogspot.com/2018/10/winsearchdbanalyzer.html|


- VM

|OS|버전|System Name|
|-|-|-|
|Windows 10 - Pro|10.0.19044 Build 19044.1826|DESKTOP-DR9PJ51|

## WinSearchDBAnalyzer
Windows.edb 기록은 안티포렌식을 극복할 수 있는 대안이 될 수 있습니다. 많은 시간정보와, 레코드를 기록하고 있으며, edb 파일을 제대로 분석하기 위해서는 다음과 같은 방법들이 있습니다.

edb파일은 ESEDatabaseView를 통해 확인 가능합니다. 그러나 본 페이지에서는 더욱더 많은 정보를 파싱해서 확인할 수 있는 도구인 WinSearchDBAnalyzer로 분석한 결과를 정리합니다.

WinSearchDBAnalyzer 오픈소스를 다운받을 수 있는 사이트에서 설명하는 내용은 다음과 같습니다.

|WinSearchDBAnalyzer 장점|
|-|
|Windows.edb에서 삭제된 기록을 복구 가능|
|Windows 10에서 사용 가능|
|라이브 시스템에서 Windows.edb를 추출하고 분석 가능|
|파일 상태에 관계없이 모든 파일을 구문 분석 가능|
|다른 도구보다 더 많은 정보를 확인 가능|
|UTC 시간에 적용 가능|


|팁|설명|
|-|-|
|http://, https://|사용자가 방문한 URL 확인하기|
|q=, query=|인터넷 검색어 확인하기|
|yyyy-mm-|특정 시간의 기록 확인하기|
|ALL|모든 기록 확인하기|
|"알 수 없음" 체크|삭제된 기록을 복구하여 확인하기|

### 사용법
![searchdb1.png](/windowsedb/searchdb1.png)

|번호|설명|
|-|-|
|1|저장되어 있는 Windows.edb 파일을 불러오기|
|2|라이브 포렌식에서 사용할 수 있는 옵션으로, 프로그램을 실행하고 있는 컴퓨터의 Windows.edb 파일을 자동으로 불러옴|
|3|1번을 설정했을 때 파일을 불러오는 창|
|4|삭제된 정보도 복구해서 파싱하는 기능|

Windows.edb의 [File]-[Open]을 클릭하면 확인할 수 있는 창입니다. 해당 창에서 위와 같은 표를 참조하여 원하는 Windows.edb 파일을 첨부합니다. 만약 Recovery 기능을 사용하지 않고 Parsing을 원한다면 "4번 왼쪽 옵션"을 선택하고 진행하면 됩니다.

![searchdb2.png](/windowsedb/searchdb2.png)

파일을 첨부한 이후에는 위와 같이 UTC를 설정하는 창이 나옵니다.

![searchdb3.png](/windowsedb/searchdb3.png)

UTC 시각을 설정한 후에는 위와 같이 파싱할 아이템을 선택할 수 있습니다. 만약 분석 대상 edb 파일이 크고, 아이템을 많이 선택한다면 프로그램이 꺼질 수 있습니다. 현재 분석하고 있는 분석 대상 아티팩트의 크기는 
72MB입니다. Host PC의 경우 Windows.edb를 삭제하지 않고 계속해서 쌓아두면 15.9GB까지 저장되기도 합니다. 원활한 분석을 위해 VM환경에서 추출한 72MB인 Windows.edb 파일을 분석합니다.

아이템의 목록을 살펴보면 매우 다양하며, 상황에 따라 분석할 내용을 파악하고 아이템을 선정해야 합니다.
위 사진처럼 Default로 설정되어 있는 Item 목록이 있고, 사용자가 임의로 추가할 수 있습니다.

![searchdb4.png](/windowsedb/searchdb4.png)

|번호|설명|
|-|-|
|1|검색 기능을 사용할 수 있습니다. ALL 폴더를 선택하여 검색을 진행하면 전체적인 검색이 가능합니다. 위 사진에서는 Outlook을 검색하여 Outlook 관련 정보를 확인하고 있습니다. 또한 앞서 설명했던 "팁"에 관한 내용의 필터링 검색도 가능합니다.|
|2|Directory Tree가 존재하는 공간입니다. ALL은 Windows.edb의 모든 정보를 나타내며, Category는 확장자로 분류된 데이터, iehistory는 Internet Explorer 관련 History, mapi16은 outlook 관련 계정과 해당 계정의 데이터, winrt는 분석 대상 PC에 존재하는 계정의 SID들에 존재하는 데이터를 표시합니다. SID에 존재하는 데이터는 Activity Data, Edge 등이 있습니다.|
|3|검색한 데이터 또는 선택한 폴더의 데이터들이 나열됩니다. 도구를 사용하기 이전에 선택했던 Item 정보가 각 데이터마다 파싱되는 것을 확인할 수 있습니다.|
|4|3번에서 설명한 데이터의 파싱 값을 y축으로 정렬한 인터페이스입니다. 선택했던 Item의 값을 불러와 정렬합니다.|

위의 사진과 표의 설명을 숙지하고 분석을 진행하면 안티포렌식을 극복할 수 있는 대안이 될 수 있고, Windows 관련 웹 브라우저, 앱 등이 남기는 흔적을 분석할 수 있습니다. 특히 Outlook 관점에서 분석을 진행한다면 사용자 이메일, 메일 정보, 일정, 연락처 등 다양한 정보를 확인할 수 있습니다.

아래는 "WinSearchDBAnalyzer" 툴을 통해서 Windows.edb를 분석했을 때 얻을 수 있는 대표적인 정보들입니다.

|Target|File Information|
|-|-|
|Outlook|MIME Type, Bcc, Cc, From, To, Summary 등의 메일 정보 등|
|IE, Edge|URL 등|
|File|FileOwner, ImageFiles 등 |
|LNK|TargetUrlPath, Arguments 등|
|ThumbnailCache|-|


## 마무리
- WinSearchDBAnalyzer 툴을 통해 Windows.edb를 분석한다면 Windows 관련 웹 브라우저, 앱 등이 남기는 정보를 확인할 수 있습니다.

## 추가 관련 참조
- Outlook: [Basic](/ko/Artifact/Outlook/Basic), [Advanced](/ko/Artifact/Outlook/Advanced)