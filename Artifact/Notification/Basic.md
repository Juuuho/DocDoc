---
title: Basic
description: 
published: true
date: 2022-12-02T11:13:07.304Z
tags: 
editor: markdown
dateCreated: 2022-11-12T14:57:26.961Z
---

# Windows Push Notification Center

Windows Notification Center는 Windows Phone 8.1에서 처음 소개되고, Windows 10 출시와 함께 PC에 도입된 아티팩트입니다.

MS Office, Slack등 사용자가 지정한 다양한 응용 프로그램의 이벤트를 화면에 표출해주고, 저장하는 기능을 수행합니다. 이를 분석한다면 사용자의 컴퓨터 사용 이력을 추적할 수 있습니다.

## Windows Push Notification Center의 경로

C:\\Users\\UserName\\AppData\\Local\\Microsoft\\Windows\\Notifications\\wpndatabase.db

![](/noti/noti.png)

해당 경로에는 wpndatabase.db (분석 대상)과 함께 Journal File도 확인할 수 있습니다.

## 추가 관련 참조

더 자세한 사항은 [Advanced](/ko/Artifact/Notification/Advanced) 에서 확인하실 수 있습니다.