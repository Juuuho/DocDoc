---
title: Slack
description: 
published: true
date: 2022-11-10T06:06:07.118Z
tags: 
editor: markdown
dateCreated: 2022-10-13T02:20:08.915Z
---

# Slack

**Slack(Searchable Log of All Conversation and Knowledge)**

Slack은 클라우드 기반 팀 협업 애플리케이션입니다. 계정은 Workspace 단위로 관리되며, Workspace는 다양하게 생성할 수 있습니다. Workspace는 이메일 또는 URL을 공유함으로써 다른 사용자를 초대할 수 있습니다. 또한, Workspace 내에서 채널을 생성할 수도 있으며, 채널의 공개 여부도 선택할 수 있습니다. 그리고 채널 내에서 단체 채팅, 일대일 채팅, 음성 통화와 사진, 동영상 및 문서 공유 등의 기능을 제공합니다.

- **본 분석정보는 PC Discord 아티팩트에 대해서만 작성되었습니다.**


--------

- 분석 목적 : slack 아티팩트를 식별하여 포렌식적으로 의미있는 데이터 선별 및 확인

- 분석 환경 : Windows 10 pro 21H2

- Analysis Files : C:\Users\{사용자}\AppData\Roaming\Slack

- 분석 도구

|도구|버전|URL|
|-|-|-|
|DB Browser for SQLite3|3.12.2|https://sqlitebrower.org|
|HxD|2.5.0.0|https://www.mh-nexus.de|
|ChromeCacheView|2.21|https://www.nirsoft.net|


## 상세분석

![slack구조_수정.png](/slack/slack구조_수정.png)


### Cache Directory

Cache 디렉터리의 데이터는 Chrome 브라우저와 Cache 파일 데이터 구조와 동일합니다. data라는 파일은 data_0~3 형태로, f 파일은 f_(0){4}([0-9|[a-z]){2} 형태로 저장됩니다. 

data_0에는 Cache 데이터 인덱스 정보가 저장되며, data_1 ~ data_3 파일은 URL, Cache 데이터를 포함합니다. 
f로 시작하는 파일은 프로필 사진이나 주고받은 사진, 다운로드 받은 파일, 동영상의 썸네일 등을 포함합니다.


- **Example1**

![f_cache_red1.png](/slack/f_cache_red1.png)

위의 그림은 분석 시에 Cache\Cache Data 폴더에 존재했던 하나의 캐시 파일입니다. File Header Signature는 pdf signature로 온전하게 기록되어 있으며, pdf로 확장자로 변환 시 문제 없이 파일을 열 수 있습니다. 해당 파일의 이름은 data_0~3 파일을 통해 확인할 수 있습니다.



- **Example2**

![chromecacheview.png](/slack/chromecacheview.png)

앞서 언급한 대로 Chrome 브라우저 Cache 파일 구조와 같기 때문에 ChromeCacheView로도 열 수 있습니다.


- **Example3**
![cache_data.png](/slack/cache_data.png)

해당 채널에 대해 웹 로그인 세션이나 PC 어플리케이션 환경에서 로그인 세션 기록이 남아있다면, Cache에 남아있는 이전에 다운로드했던 URL 기록을 입력할 시에 파일 다운로드가 또 가능해집니다.


### IndexedDB Directory

![leveldb.png](/slack/leveldb.png)

IndexedDB\http_app.slack.com_0.indexeddb.leveldb 디렉토리에는 client가 지속적으로 활동하면서 생성되는 로그가 기록됩니다.

### Local Storage Directory

![local_storage.png](/slack/local_storage.png)

Local Storage\leveldb 경로에 존재하는 로그 파일에는 lastlogged라는 마지막 로그인 정보와 lastactivity라는 마지막 활동 시간 정보 등이 저장되어 있습니다. 시간 정보는 Unix Time 형태로 저장됩니다.

![local_storage_name.png](/slack/local_storage_name.png)

또한, leveldb 폴더 내에 ldb파일에서 Workspace ID와 이름정보를 확인할 수도 있습니다.

### Logs Directory

![logs_default.png](/slack/logs_default.png)

위 그림은 logs\default\ 폴더에 browser.log에 남는 기록입니다.

  "id": 1,
  "url": "https://app.slack.com/client/T03EWTCQU0H/C03RMN6EHL5"
  
[09/28/22, 15:27:43:308] info: Breadcrumb: electron: window.scroll-touch-edge

browser.log 파일에는 언제 URL에 접속했고 어떤 행위를 했는지 info 정보가 남습니다. 분석 시 실제로 사용하던 슬랙 채널에 기반하여 살펴보면 T03EWTCQU0H는 DFC 2022라는 Workspace 이름을 의미하고, C03RMN6EHL5는 Workspace 내에서 402 라는 채널을 의미합니다. 
Slack은 이처럼 client가 브라우징한 모든 것들을 URL화 해서 server인 slack-service로 전송합니다. 


- **Slack Manual**
![slack_매뉴얼.png](/slack/slack_매뉴얼.png)

실제로 Slack 기능 매뉴얼에도 나와있듯이, Slack은 PC Application인 client와 Slack server인 service간에 데이터 전송이 이루어지고, 전송은 암호화로 진행된다는 것을 알 수 있습니다.



### Storage Directory

![root-state.png](/slack/root-state.png)

위 그림은 Storage 폴더에 존재하는 root-state.json 파일을 HxD로 열었을 때 확인할 수 있습니다. Slack에 로그인한 사용자가 다운받았던 파일에 대해 URL로 기록되는 것을 알 수 있습니다. 이는 웹 로그인 세션이나 PC Application 로그인 세션이 유지되고 있을 때, 기록된 URL에 접속하면 해당 파일을 다운로드 받을 수 있습니다. 
또한, 파일을 다운로드한 경로(downloadpath)와 다운로드 시각(starttime-endtime)도 위 그림에서 밑 부분을 통해 확인할 수 있습니다.


따라서, 분석 대상 PC의 root-state.json 파일 내에 기록된 정보를 토대로 Slack 계정 로그인 없이도 PC 사용자가 Slack에 로그인 해서 파일을 다운로드 받았다면 어떤 경로에, 어떤 파일을 언제 다운로드 받았는지 알 수 있습니다.

## 마무리
- **Slack 아티팩트를 통해서 사용자 계정정보 로그인 없이도 로그가 남아있다면 파일 다운로드 경로, 파일 다운로드 시각, 파일 이름 등을 확인할 수 있습니다.**
- **또한, 활동했던 워크스페이스 ID, 이름, User와 매칭되는 ID 역시 확인할 수 있습니다.**

## 추가 관련 참조
- 디지털 포렌식 