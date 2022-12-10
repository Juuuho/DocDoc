---
title: Discord
description: 
published: true
date: 2022-11-10T06:01:00.780Z
tags: 
editor: markdown
dateCreated: 2022-10-13T02:21:46.133Z
---

# Discord

Discord는 초기에 게이머를 위해 출시된 메신저 및 VoIP 애플리케이션입니다. 여러 사용자를 묶어 하나의 서버로 만들 수 있으며, 서버의 초대 코드를 생성하여 원하는 사용자를 초대할 수도 있습니다. 

서버 생성과 참여가 간편해서 비공개 그룹의 서버를 중심으로 범죄 활동을 수행하는 악의적인 사용자들의 소통 수단이 되고 있습니다.

- **본 분석정보는 PC Discord 아티팩트에 대해서만 작성되었습니다.**

## 분석 기본 정보

- 분석 목적 : Discord 아티팩트를 식별하여 포렌식적으로 의미있는 데이터 선별 및 확인

- 분석 환경 : Windows 10 pro 21H2

- Analysis Files : C:\Users{사용자}\AppData\Roaming\Discord

- 분석 도구

|도구|버전|URL|
|-|-|-|
|DB Browser for SQLite3|3.12.2|https://sqlitebrower.org|
|HxD|2.5.0.0|https://www.mh-nexus.de|
|ChromeCacheView|2.21|https://www.nirsoft.net|
|leveldb-viewer|-|https://github.com/markmckinnon/Leveldb-py|


## 상세분석


![discord구조1.png](/discord/discord구조1.png)

Discord 역시 대부분의 application에서 보이는 구조와 비슷합니다.
하지만 대부분의 폴더에서 기록된 로그는 다른 application과 다르게 정보를 HxD로 열어서 보기에는 한계가 있어 유의미한 데이터를 찾기가 어렵습니다.

유의미하게 볼 수 있는 폴더는 Cache, Local Storage\leveldb, old_cache_000 가 있습니다.

### Cache 파일

Cache 정보는 Chrome Cache 구조와 동일합니다. 따라서, ChromeCacheview로 Cache 파일을 살펴볼 수 있습니다.

![chromecache.png](/discord/chromecache.png)

하지만, old_cache_000 폴더 내 존재하는 cache 파일은 ChromeCacheview로 확인할 수 없습니다. 따라서, 파일 정보를 일일히 확인해야 하는 단점이 있습니다.

data라는 파일은 data_0~3 형태로, f 파일은 f_(0){4}([0-9|[a-z]){2} 형태로 저장됩니다.

### leveldb

![leveldb.png](/discord/leveldb.png)

Local Storage\leveldb에 남는 log파일과 ldb파일 중 ldb파일은 leveldb-viewer나 leveldb-parser로 로그를 확인하면 어느정도 유의미한 데이터를 얻을 수 있습니다.

data는 json형태로 존재하고, key에 따라 value에 적히는 정보는 다양합니다. 또한, value안에서도 key:value형태가 존재합니다.

DB상에서 주요 key에 대한 value값들을 분석했던 파일의 예시를 통해 살펴보겠습니다.

- **Example**

|key|value|
|-|-|
|SelectedChannelStore|selectedChannelID, selectedVoiceChannelID, lastConnectedTime|
|RTCRegionStore|prefferedRegion,lastTestTimestamp,lastGeoRankedOrder|
|UserAffinitiesStore|userID, affinity|
|GuildAffinitiesStore|guildID, score|


정보가 직관적이지는 않지만 살펴보았던 채널 ID, 마지막으로 연결된 시각, 가장 최근에 선택한 채팅채널 ID, 선호 나라 등을 알 수 있습니다.

다음으로는 Value가 어떤 형태로 저장되는지 살펴보겠습니다.

- **Example**

|key|value|
|-|-|
|selectedChannelID|1014184928814043177|
|lastConnectedTime|1664944981069|
|preferredRegion|south-korea|
|lastGeoRankedOrder|south-korea,japan,hongkong,singapore|
|userID|951693459037761567|


lastConnectedTime은 unix timestamp형태로 저장됩니다.


## 마무리
- PC Discord 아티팩트에서는 직관적으로 알 수 있는 정보는 다른 Application에 비해 많이 없었습니다.
- Discord는 모바일 상의 아티팩트에서 더 많은 정보를 확인할 수 있습니다. 따라서 모바일 아티팩트를 분석하는 것이 적절해 보입니다.
- 분석 대상 PC 사용자가 주로 활동하는 채널 기반으로 나라 정보를 얻거나, 특정 채널 ID를 가진 채널에서 마지막으로 연결했던 시간 정보, 특정 숫자로 저장된 userID 정보 정도를 알 수 있습니다.


## 추가 관련 참조
- https://www.researchgate.net/publication/347044759_Digital_Forensic_Acquisition_and_Analysis_of_Discord_Applications
- 디지털 포렌식 관점에서의 Slack 및 Discord 메신저 아티팩트 분석, 디지털콘텐츠학회논문지, Vol.21, No. 4, pp. 799-809, Apr. 2020
