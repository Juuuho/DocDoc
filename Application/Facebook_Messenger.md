---
title: Facebook_Messenger
description: 페이스북 메신저
published: true
date: 2022-11-10T09:11:54.946Z
tags: 
editor: markdown
dateCreated: 2022-11-09T11:47:10.420Z
---

# Facebook Messenger

Facebook Messenger는 Facebook을 사용하는 사용자라면 대부분 사용해본적 있는 메신저입니다. Facebook Messenger는 대부분 모바일 환경에서 사용되지만, PC 버전에서 사용했을 때 어떠한 정보가 남는지 관련 아티팩트를 분석해봅니다.

- 분석 목적 : 메신저 앱인 Facebook Messenger에서 얻을 수 있는 유의미한 정보를 확인

- 분석 환경 : Windows 10 pro 21H2

- Analysis Files : C:\Users\{사용자}\AppData\Local\Messenger

> Facebook Messenger는 대부분의 Application이 아티팩트를 Roaming 경로에 저장하는 것과는 달리, Local 폴더에 유의미한 아티팩트가 저장됩니다.

- 분석 도구

|도구|버전|URL|
|-|-|-|
|DB Browser for SQLite|3.12.2|https://www.sqlitebrowser.org|
|Hxd|2.5.0.0|https://www.mh-nexus.de|


## 상세분석

- **Messenger 폴더 내 파일**

![구조.png](/messenger/구조.png)

Facebook Messenger 설치 시 사용자 데이터가 저장되는 경로는 C:\Users\{사용자}\AppData\Local\Messenger입니다.

폴더 내에서 유의미하게 분석할 수 있는 파일은 msys_[userid].db 파일입니다.

userid는 15자리의 숫자로 이루어져있습니다.

- **Table 정보**
![table수.png](/messenger/table수.png)

db 파일을 열어보면 엄청 많은 테이블 개수를 확인할 수 있습니다.


주로 살펴볼 수 있는 테이블을 알아보도록 하겠습니다.

- **현재 활동중인 사용자**
![현재_활동중인_사용자_top.png](/messenger/현재_활동중인_사용자_top.png)

_top_active_now_contacts 테이블에서 알 수 있는 정보는 다음과 같습니다.

|Column|Info|
|-|-|
|contact_id|활동중인 사용자의 uid|
|profile_picture_url|프로필 사진 URL|
|name|활동중인 사용자 이름|

페이스북 메신저에서는 페이스북 상에서 활동중인 사용자 정보를 확인할 수 있습니다. 

![마지막으로활동한시각.png](/messenger/마지막으로활동한시각.png)

그리고 사용자가 마지막으로 활동한 시각이 UTC timestamp로 기록되기도 합니다.

- **message정보**
![message정보.png](/messenger/message정보.png)

message 테이블에서 확인할 수 있는 정보는 다음과 같습니다.

|Column|Info|
|-|-|
|thread_key|대화 상대의 id|
|timestamp_ms|대화 시각|
|text|대화 내용|
|sender_id|송신자 id(사용자 or 상대 사용자)|

메시지 정보는 그림에서 보이는 text와 같이 Messenger에 로그인한 사용자가 다른 사용자와 나누었던 채팅 기록을 확인할 수 있습니다.
별다른 암호화과정 없이 해당 text를 보낸 시각과 송신자 id정보를 알 수 있어서 누가 언제 해당 채팅을 보냈는지 확인이 가능합니다. 사용자가 PC에서 로그인을 하게 되면 이전에 어떤 기기에서 messenger 어플리케이션을 통해 채팅을 나누었던간에 모든 기록을 다 불러오게 되어서 몇 년 전에 보냈던 대화까지 확인할 수 있습니다.



- **첨부파일 정보**
![첨부파일정보.png](/messenger/첨부파일정보.png)

attachment 테이블에서 확인할 수 있는 정보는 다음과 같습니다.

|Column|Info|
|-|-|
|thread_key|파일을 첨부한 사용자의 id|
|timestamp|파일을 업로드 한 시각|
|filename|정형화 된 파일 이름|
|filesize|파일 크기|
|is_sharable|공유 여부|
|playable_url|활성화된 URL|

대부분 파일에 관해서 유의미한 정보를 얻을 수 있습니다. 특히, playable_url은 파일에 대한 활성화된 url입니다. 따라서, url에 접속했을 때 파일이 이미지 형태면 이미지 정보를 알 수 있고, 파일 형태면 파일이 다운로드됩니다.


- **내 프로필정보**
![self_profile.png](/messenger/self_profile.png)

self_profile 테이블에서는 메신저에 로그인한 사용자에 대한 프로필 정보를 확인할 수 있습니다.


- **연락처 정보**
![contact.png](/messenger/contact.png)

contacts 테이블에서 확인할 수 있는 정보는 다음과 같습니다.

|Column|Info|
|-|-|
|id|사용자와 친구되어 있는 사용자의 ID|
|profile_picture_url|프로필 사진 정보|
|name|사용자 이름(성+이름)|
|first name|이름|
|is_messenger_user|메신저 사용자인지 여부|

contacts에서는 대상 PC 메신저에 로그인한 사용자와 친구되어 있는 사용자들을 알 수 있는 연락처 테이블입니다.
해당 테이블에서는 친구 이름과 친구 프로필 사진, 그리고 Facebook Messenger 사용자인지 등을 알 수 있습니다.




## 마무리
- 현재 국내에서 Facebook Messenger를 사용하는 사람은 많이 줄어들었지만, PC 버전으로 메신저를 이용해서 대화할 시에 모든 정보가 DB에 저장되기 때문에 보안적인 측면에서 각별한 주의가 필요해 보입니다.
- 분석가 입장에서는 분석 대상 PC 사용자가 facebook messenger를 사용한다면 유의미한 정보를 많이 얻을 수 있습니다. 


## 추가 관련 참조
