---
title: Advanced
description: 
published: true
date: 2022-11-25T06:03:34.808Z
tags: 
editor: markdown
dateCreated: 2022-11-12T14:58:02.688Z
---

# Windows Push Notification

- 분석 목적 : Windows Push Notification 아티팩트를 디지털 포렌식 관점에서 살펴보기 위함

- 분석 환경 : Windows 10 pro 21H2

- Analysis Files : C:\Users\사용자\AppData\Local\Microsoft\Windows\Notifications

- 분석 도구

|도구|버전|URL|
|-|-|-|
|DB Browser for SQLite|3.12.2|https://www.sqlitebrowser.org|


## 상세분석

wpndatabase.db 파일을 DB Browser for SQLite로 열어보았을 때, 디지털 포렌식 관점에서 유의미하게 확인할 수 있는 테이블은 Notification 테이블입니다.

![wpn_1_수정.png](/wpn/wpn_1_수정.png)

알림은 주로 배지, 타일, 및 토스트 형태로 존재합니다. 실제로 Notification table에서도 type 부분을 살펴보면 badge, tile, toast가 존재하는 것을 위 그림을 통해 알 수 있습니다. 세 타입은 그래픽적인 출력이 존재하는 타입이며 이 외로 존재하는 raw type은 그래픽적인 출력이 존재하지 않습니다.

본 문서에서는 badge, tile, toast 알림 중 toast에 대해서 중점적으로 살펴보도록 하겠습니다.

### Toast 

![wpn_2.png](/wpn/wpn_2.png)

윈도우즈 오른쪽 하단에 팝업으로 떴다가 사라지는 해당 정보는 Windows Push Notification의 Toast 메시지입니다. 

![wpn_3.png](/wpn/wpn_3.png)

위 그림의 데이터베이스 상에서도 toast 메시지 정보 확인이 가능합니다.

![wpn_메일.png](/wpn/wpn_메일.png)

토스트 메시지의 정보 확인을 통해 디지털 포렌식 관점에서 분석 대상 PC 사용자의 활동 조사도 가능하지만, 알림에 대한 도착 시간 또한 중요합니다.
Windows Push Notification에서 이메일 클라이언트 알림 메시지가 수신되었을 때 언제 수신되었는지 조사할 수 있는 중요한 요소로 작용할 수 있습니다.

- **Toast Content**

토스트 메시지의 payload field 내 존재하는 내용은 다음과 같습니다.

![wpn_내용.png](/wpn/wpn_내용.png)

duration은 토스트 메시지가 알림으로 뜨는 시간으로 5초로 짧은 경우 short로 기록되었습니다.
launch는 알림을 유발하는 애플리케이션에 따라 달라집니다. 위 그림같은 경우는 Windows Defender 알림이기 때문에 "windowsdefender://enablertp" 로 기록이 된 것을 확인할 수 있습니다.
windows 기본 시스템에 대한 알림은 windows defender나 outlook(launch:outlookmail)처럼 기록되지만,
기타 애플리케이션은 다르게 기록됩니다.

- **Group**

![wpn_4_수정.png](/wpn/wpn_4_수정.png)

위 그림의 오른쪽 부분에서 MailGroup과 TOAST_BUDS라는 키워드를 볼 수 있습니다. 
outlook에서 온 메일은 grouping 하여 MailGroup으로, bluetooth로 Galaxy Buds+ 연결 요청 여부는 Toast_BUDS로 grouping된다는 사실을 알 수 있습니다.

![wpn_5.png](/wpn/wpn_5.png)
기본적으로 알림 메시지는 5초 동안 표시되지만 Windows 접근성 - 알림 - 디스플레이에서 최대 5분까지 표시가 가능합니다.



### Notification Handler

![wpn_7.png](/wpn/wpn_7.png)

Windows Push Notification에서 Notification Handler 테이블에서는 해당 알림을 Control 하는 주체(애플리케이션 ID)들을 나열합니다.
Handler Type 필드는 badge, tile, toast 및 toastCondensed 한정자 중 하나를 포함할 수 있는 알림 유형을 나타내는 텍스트 필드입니다.


### Metadata

![wpn_8.png](/wpn/wpn_8.png)

metadata 테이블은 기본적으로 badge, tile, toast에 대한 maxCount값이 존재합니다. 이 Value값은 Notification에서 주어진 시간에 존재할 수 있는 최대 알림 수를 의미합니다. 기본적으로 사용자의 Notification table에서 존재하는 개수는 지정된 Value값에 대해서 다를 수는 있습니다.

### Wpnidm 디렉터리

![wpnidm.png](/wpn/wpnidm.png)

Notification 폴더 내에는 Wpnidm이라는 폴더가 존재합니다. 해당 디렉터리에는 JPG, PNG 및 IMG 이미지를 보유합니다. 이미지들은 토스트 알림 및 타일에서 가져옵니다.
예를 들어, Windows 용 Facebook Messenger application에서는 발신자 facebook 계정의 프로필 사진 축소판을 썸네일로 가져와 wpnidm 디렉토리에 보관합니다. 이때 보관되는 파일의 이름은 4바이트의 hex값으로 저장됩니다.


## 마무리
- Windows Push Notification은 응용 프로그램과 로그온 한 사용자에게 다양한 유형의 알림을 제공하는 Windows 10 서비스입니다.
- 알림의 유형은 주로 badge, tile, toast형태로 존재합니다.
- Toast메세지의 내용이나 도착 시간 등을 통해 디지털 포렌식 조사에 유용하게 사용될 수 있습니다.
- Push Notification 아티팩트의 디지털 포렌식 가치는 설치된 애플리케이션과 분석 중인 컴퓨터에서 사용자의 애플리케이션 사용에 따라 달라집니다.


## 추가 관련 참조
- WPN 분석과 autopsy WNA module 이용 : https://www.mdpi.com/2673-6756/2/1/7
