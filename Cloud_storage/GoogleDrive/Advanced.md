---
title: Advanced
description: GoogleDrive_Advanced
published: true
date: 2022-10-28T03:41:14.582Z
tags: cloud storage, googledrive
editor: markdown
dateCreated: 2022-10-26T07:38:55.089Z
---

# Google Drive

- 분석 목적 : 구글 드라이브를 사용하는 사용자의 PC로 부터 사용흔적을 수집하고, 중요한 파일이 존재할 경우 획득하기 위함.

- 분석 환경 : Windows 10 pro 버전 21H2(OS 빌드 19044.2130)

- Analysis Files : 로컬 캐시 폴더

- 분석 도구

|도구|버전|URL|
|-|-|-|
|FTK Imager|4.7.1.2|https://accessdata.com/product-download/ftk-imager-version-4-5|
|DB Browser for SQLite|3.12.2|https://sqlitebrowser.org/|
|로그 분석기|2|https://toolbox.googleapps.com/apps/loggershark/|

- VM

|OS|버전|
|-|-|
|Windows 10 Pro|21H2 (OS 빌드 191206.1406)|

## 상세분석
### 설치 로그 (InstallLog)
![installlog.png](/cloudstorage/googledrive/installlog.png)
**HKEY_LOCAL_MACHINE\SOFTWARE\Google\DriveFS** 레지스트리에서는 사용자가 구글 드라이브를 설치한 로그가 기록된 파일의 경로를 확인할 수 있으며, 해당 로그파일에서는 설치를 시작한 시간/설치 과정/설치가 종료된 시각까지 알 수 있습니다.

### TrustedRootCertsFile
![rootspem.png](/cloudstorage/googledrive/rootspem.png)
**%ProgramFiles%\Google\Drive File Stream\\{version}\config\roots.pem**
구글의 설명에 따르면, "데스크톱용 Drive는 MITM(Man-In-The-Middle)공격으로부터 보호하기 위해 모든 네트워크 트래픽을 암호화하고 호스트 인증서의 유효성을 검사합니다. 복호화 프록시를 사용하는 네트워크에 배포하는 경우 데스크톱용 Drive에 TrustedRootCertsFile 설정을 구성해야 합니다."라고 안내하고 있습니다.

따라서, 사용자가 MITM 공격을 받고 있거나 Proxy SG와 같은 제품 혹은 소프트웨어를 사용하고 있는 경우 구글 드라이브 애플리케이션의 SSL 인증서 유효성 검사로 인해 동기화가 중단될 수 있습니다.

### experiments.db
![experiments.db.png](/cloudstorage/googledrive/experiments.db.png)
%UserProfile%\AppData\Local\Google\DriveFS\
해당 파일에서는 구글 드라이브를 사용하는 계정의 account_ids값과 last_sync값을 확인할 수 있습니다.

### root_preference_sqlite.db
![root_preference_sqlite.db.png](/cloudstorage/googledrive/root_preference_sqlite.db.png)
해당 파일은 분석대상 PC의 드라이브에 대한 정보를 담고 있습니다.
외부저장장치와 구글 드라이브 정보 또한 담고 있으며, 드라이브의 GUID와 마지막 마운트 드라이브 문자, 드라이브 크기(byte)를 확인할 수 있습니다.

### content_cache
![content_cache.png](/cloudstorage/googledrive/content_cache.png)
%USERPROFILE%\AppData\Local\Google\DriveFS\{account_ids}\content_cache
해당 폴더는 사용자가 구글 드라이브의 파일을 읽거나 쓰는 등 상호작용이 발생할 경우 파일이 임시로 저장되는 경로입니다.
content_cache 폴더에는 d{숫자}형식의 폴더들이 존재하는데, 이곳들에 임시파일이 저장됩니다.
저장되는 파일명과 확장자는 기재되지 않지만, 파일의 Hex값은 그대로 남아있어 시그니처 등을 토대로 올바른 확장자를 찾을 수 있으면 원본 파일을 쉽게 확인할 수 있습니다.

### metadata_sqlite_db
![metadata_sqlite_db.png](/cloudstorage/googledrive/metadata_sqlite_db.png)
%USERPROFILE%\AppData\Local\Google\DriveFS\\{account_ids}\metadata_sqlite_db
해당 파일은 구글 드라이브와 동기화된 파일과 폴더들에 대한 내용을 담고 있습니다.

### DriveFS LOG
%USERPROFILE%\AppData\Local\Google\DriveFS\Logs\
해당 폴더에는 구글 드라이브 활동에 대한 전반적인 로그가 존재하며 지원 및 진단, 문제해결을 위해 생성되는 로그를 담고 있습니다.
또한, 구글에서는 생성된 로그를 직접 진단하기 위한 [로그 분석기](https://toolbox.googleapps.com/apps/loggershark/)를 제공하고 있습니다.

![drivefs.png](/cloudstorage/googledrive/drivefs.png)
로그들은 drive_fs.txt 혹은 drive_fs_{숫자}.txt 형식의 파일명을 갖고 있으며, parent_{숫자}.txt 파일과 마찬가지로 재부팅 처럼 프로세스가 시작될때마다 숫자가 1씩 증가하여 생성됩니다.

구글에서 제공하는 로그 분석기를 사용하면 다음과 같은 정보를 얻을 수 있습니다.

![로그분석기1.png](/cloudstorage/googledrive/로그분석기1.png)
구글 드라이브를 사용하는 사용자의 인증 시간과 구글 계정 등 사용자에 대한 정보(62줄)와 해당 사용자의 account_ids(63줄, 1016605...)를 알 수 있습니다.

![로그분석기2.png](/cloudstorage/googledrive/로그분석기2.png)
244줄부터 보게되면 볼륨이 추가되고 특정 account_ids값과 매칭되는 계정이 어느 볼륨과 마운트 되었는지 알 수 있습니다.

![로그분석기3.png](/cloudstorage/googledrive/로그분석기3.png)
또한, 사용자의 PC의 구글 드라이브에 동기화되었다가 삭제된 계정의 경우 매칭되는 accounts_ids 값을 검색하여 삭제된 시간을 확인할 수 있습니다.

### PID
%USERPROFILE%\AppData\Local\Google\DriveFS\pid.txt
해당 파일에서 구글 드라이브 데스크톱앱이 마지막으로 실행된 시점의 PID를 확인할 수 있습니다.


## 마무리
1. 구글 드라이브는 크게 두 가지 방법(파일 스트림, 파일 미러링)으로 서비스를 제공합니다.
2. 파일 스트림 방식의 경우 한번이라도 파일과의 상호작용이 있었다면 로컬 캐시 폴더에 흔적이 남아 원본 파일 획득이 가능합니다.
3. 미러링 방식은 사용자의 폴더에 존재함으로 수집, 분석이 용이합니다.
4. 구글 드라이브는 진단 및 지원용 로그를 수집하고 있으며, 구글에서 제공하는 로그 분석기를 통해 직접 로그 분석이 가능합니다.
5. 계정정보를 account_ids라는 토큰으로 관리하고 있으며, 해당 토큰값으로 사용자의 행위를 추적할 수 있습니다.
6. 다만 파일 업/다운로드 로그는 난독화가 되어있어 정확한 정보를 얻기는 어렵습니다.
## 추가 관련 참조
- Registry: [Registry](/ko/Artifact/Registry)