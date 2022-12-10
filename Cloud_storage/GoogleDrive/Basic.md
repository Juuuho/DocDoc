---
title: Basic
description: GoogleDrive_Basic
published: true
date: 2022-12-02T12:00:37.805Z
tags: cloud storage, googledrive
editor: markdown
dateCreated: 2022-10-19T02:14:57.229Z
---

> **분석대상 환경**
> Windows 10 pro 버전 21H2(OS 빌드 19044.2006)
{.is-info}


# 동기화 옵션
구글 드라이브는 사용자에게 크게 두 가지의 방법으로 동기화 서비스를 제공하고 있습니다.

|동기화 옵션|옵션 내용|
|-|-|
|**파일 스트림**|모든 내 Drive 파일을 클라우드에만 저장합니다|
| |컴퓨터의 가상 드라이브 또는 폴더에 있는 파일에 액세스할 수 있습니다|
| |오프라인에서 사용하려는 파일 및 폴더를 지정합니다|
|**파일 미러링**|모든 내 Drive 파일을 클라우드 및 컴퓨터에 저장합니다|
| |컴퓨터의 폴더에 있는 파일에 액세스합니다|
| |모든 파일이 자동으로 오프라인에서 사용 가능합니다|
이외에도 폴더를 오른쪽 클릭하여 "폴더 동기화 또는 백업" 옵션을 사용하여 구글 드라이브에 동기화하여 사용할 수 있습니다.

# 구글 드라이브 기본정보

`HKEY_USERS\\{SID}\SOFTWARE\Google\DriveFS\Share`
![레지스트리1.png](/cloudstorage/googledrive/레지스트리1.png)
해당 레지스트리에서는 BasePath와 SyncTargets 정보에 대해 알 수 있습니다.

## BasePath
![basepath.png](/cloudstorage/googledrive/basepath.png)
BasePath 경로에 존재하는 폴더에는 구글 드라이브 사용기록과 캐시파일 등 구글 드라이브 사용에 대한 전반적인 내용이 기록되거나 저장되어 있습니다.
> BasePath 경로는 일반적으로 사용자가 직접 정의하지 않는 한 "%UserProfile%\AppData\Local\Google\DriveFS"로 되어있습니다.
{.is-info}

![basepath2.png](/cloudstorage/googledrive/basepath2.png)
DriveFS 폴더의 구조는 위와 같으며, 사진 상 제일 위에 위치하는 101660570451790904737 폴더의 경우 사용자PC에서 구글 드라이브를 사용하는 계정들의 account_ids를 의미하며, 계정별 설정과 캐시파일들이 존재합니다.
구글 드라이브 데스크톱 앱의 경우 계정을 최대 4개까지 연결할 수 있습니다.
따라서, DriveFS폴더에서 account_ids폴더는 1개에서 4개까지 존재할 수 있습니다.

## SyncTargets
![synctargets.png](/cloudstorage/googledrive/synctargets.png)
SyncTargets에서는 account_ids와 매칭되는 마운트 드라이브 문자를 확인할 수 있으며, 폴더를 우클릭하여 "폴더 동기화 또는 백업"을 진행한 폴더가 존재할 경우, 이곳에 위의 사진과 같이 존재하게 됩니다.

## 추가 관련 참조
더 자세한 사항은 [Advanced](/ko/Cloud_storage/GoogleDrive/Advanced) 에서 확인하실 수 있습니다.