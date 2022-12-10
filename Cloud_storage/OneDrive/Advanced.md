---
title: Advanced
description: OneDrive_Advanced
published: true
date: 2022-11-25T04:20:30.741Z
tags: 
editor: markdown
dateCreated: 2022-10-26T10:34:58.925Z
---

# OneDrive

- 분석 목적 : 사용자가 OneDrive 활동한 흔적을 찾아내기 위함.

- 분석 환경 : Windows 10 pro 21H2 19044.2130

- Analysis Files : OneDrive

- 분석 도구

|도구|버전|URL|
|-|-|-|
|FTK Imager|4.7.1.2|https://accessdata.com/product-download/ftk-imager-version-4-5|

- VM

|OS|버전|System Name|
|-|-|-|
|Windows 10 Pro|10.0.19044 Build 19044.1288|DESKTOP-M3J1EH3|

## 상세분석

### 상태 정보
![상태세가지.png](/cloudstorage/onedrive/상태세가지.png)
OneDrive 동기화는 파일의 상태가 크게 세가지의 상태로 나누어집니다.
![상태예시.png](/cloudstorage/onedrive/상태예시.png)
온라인 전용, 이 디바이스에서, 항상 사용 가능 세가지의 상태를 구현하고 진행하였으며, 각각의 상태를 FTK Imager를 통해 Hex 값을 보면 아래와 같은 차이점이 존재합니다.

#### 온라인 전용
![파일상태-온라인.png](/cloudstorage/onedrive/파일상태-온라인.png)
**온라인 전용** 파일의 경우 사용자가 열거나 사용하지 않은 파일을 의미하며, 파일명과 파일의 형태는 존재하지만 Hex값이 0으로 채워져 있습니다.

#### 이 디바이스에서
![파일상태-이디바이스에서.png](/cloudstorage/onedrive/파일상태-이디바이스에서.png)
**이 디바이스에서** 파일의 경우 사용자가 열거나 사용한 파일을 의미하며, 온전한 파일로 존재합니다.

#### 항상 사용 가능
![파일상태-항상사용가능.png](/cloudstorage/onedrive/파일상태-항상사용가능.png)
**항상 사용 가능** 파일의 경우 **이 디바이스에서** 파일과 동일하게 존재합니다.

### 개인 중요 보관소
![개인중요보관소.png](/cloudstorage/onedrive/개인중요보관소.png)
OneDrive에서는 개인 중요 보관소를 제공합니다. 해당 보관소를 사용하기 위해서는 동일한 Microsoft 계정에 등록된 이메일을 통해 코드 인증을 거쳐야 하며 일정 시간이 지난 뒤에는 자동으로 비활성화 됩니다.

또한, 보관소 잠금해제 직후 PC를 종료하고 이미징을 진행하게 되면 보관소를 확인할 수 없으며, 잠금해제 직후 Suspend 상태로 전환시에도 보관소를 확인할 수 없습니다.

### OneDrive 설정 정보
![local.png](/cloudstorage/onedrive/local.png)
**%UserProfile%\AppData\Local\Microsoft\OneDrive**경로에서는 사용자의 OneDrive 설정을 포함한 정보가 존재합니다.

#### 사용자 프로필
![프로필.png](/cloudstorage/onedrive/프로필.png)
위의 경로에서 **settings\Personal\{id}-ProfileServiceResponse.txt** 파일을 보면 OneDrive와 연결된 사용자의 이름, 이메일 등 계정 정보에 대해 간략하게 확인할 수 있습니다.

#### OneDrive 동기화 폴더
![폴더경로.png](/cloudstorage/onedrive/폴더경로.png)
사용자는 동기화 폴더를 변경할 수 있어 분석 시에 주의해야합니다.
따라서, **settings\Personal\{id}.ini**파일에서는 위의 사진과 같이 사용자가 지정한 OneDrive 동기화 폴더 경로가 기록되어 있습니다.

#### Log 정보
![log.png](/cloudstorage/onedrive/log.png)
Microsoft에서는 사용자에게서 수집한 텔레메트리를 서버에 전송하며, 이때 파일/폴더 이름, 사용자 이름등을 난독화하여 전송합니다. 관련 파일들은 **%UserProfile%\AppData\Local\Microsoft\OneDrive\logs**에서 확인할 수 있습니다.
## 마무리
1. Windows 10에서는 OneDrive가 기본 앱으로 제공되며, 기본적으로 사용자 폴더의 하위 폴더로 동기화 폴더를 생성하여 관리합니다.
2. 기존 OneDrive의 파일/폴더와 함께 사용자의 바탕화면, 문서, 사진 폴더를 백업합니다.
3. 이미징 시점 이전에 사용자가 OneDrive에서 파일을 한번이라도 열어보았다면 이미징 시에 해당파일을 정상적으로 가져올 수 있습니다.
4. 개인 중요 보관소의 경우 기기 잠금과 보관소 잠금이 해제되어 있는 상태가 아닌 이상 확인이 불가합니다.
5. OneDrive와 관련된 텔레메트리 로그가 존재하지만, 평문으로 되어있지 않습니다.

## 추가 관련 참조
- X