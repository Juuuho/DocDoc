---
title: Kakao
description: Kakao.exe
published: true
date: 2022-10-28T03:18:32.476Z
tags: 
editor: markdown
dateCreated: 2022-10-13T02:20:55.386Z
---

# KaKao Talk
카카오톡(Kakao Talk)은 주식회사 카카오가 2010년 3월 18일 서비스를 시작한 글로벌 모바일 인스턴스 메신저입니다. OS에 맞는 앱스토에서 앱으로 다운이 가능하고, PC버전 카카오톡도 설치가 가능합니다. 

- 분석 목적 : 해당 페이지는 Windows OS(버전 10)상에 남을 수 있는 "PC 버전 카카오톡" 기록에 대한 정보를 확인합니다.

- 분석 환경 : Microsoft Windows 10 Pro(build 19042) 64-bit

- Analysis Files : Registry, Kakao 폴더

- 분석 도구 :

|도구|버전|URL|
|-|-|-|
|Sublime Text 3|3.2|https://www.sublimetext.com/3|

- VM : 없음



## 상세분석
해당 페이지에서 다루는 정보는 "PC 버전 카카오톡(이하 Kakao.exe)" 설치 시 Windows상에 남는 아티팩트입니다. 크게 두가지로 나뉘며 하나는 Registry 값, 다른 하나는 AppData에 남는 Kakao 폴더입니다.

### Registry
- 경로 : 컴퓨터\HKEY_USERS\\[SID]\SOFTWARE\Kakao

#### UserAccounts
![계정정보모자이크.jpg](/카카오톡/계정정보모자이크.jpg)

Kakao.exe를 설치하면 생기는 레지스트리입니다. 레지스트리 값을 확인하기 위해서는 설치를 진행한 Windows OS상의 계정의 SID를 확인해야 합니다. 
위의 사진처럼 [kakao] - [Kakao Talk] - [UserAccounts] 경로에 Kakao.exe로 접속했던 ID(e-mail)가 기록됩니다. 

![내pc인증받기.png](/카카오톡/내pc인증받기.png)
단, "자동로그인", "내 PC 인증받기"를 진행한 ID만 확인이 가능합니다. 추가적으로 PC 인증을 받을 때 "내 PC 인증받기"를 제외한 "1회용 인증 받기"를 진행하면 기록이 남지 않습니다.

![카카오_계정_정보_추가.jpg](/카카오톡/카카오_계정_정보_추가.jpg)
위 사진은 카카오톡 계정 정보를 추가한 사진입니다. "1회용 인증 받기"를 진행했을 때 나오지 않았던 계정 정보가 "내 PC 인증받기"를 통해 등록됐습니다.

마지막으로 [모바일 카카오톡] - [기기 연결 관리] - [인증 기기 관리] - [인증 받은 PC 목록]에서 계정이 등록된 PC 정보를 제거하더라도 기록이 유지됩니다.

#### AdFit
![adfit.png](/카카오톡/adfit.png)
각 ID Cookie 값의 Web ID, Domain, 만료기간 등이 저장되어 있습니다.

#### DeviceInfo
![deviceinfo.png](/카카오톡/deviceinfo.png)
![deviceinfo2.png](/카카오톡/deviceinfo2.png)
자신의 UUID 값을 확인할 수 있습니다(기기 정보 포함).

#### Update
![update.png](/카카오톡/update.png)
Kakao 마지막 패치 날짜, 패치 정보 등을 확인할 수 있습니다.

### AppData
Kakao.exe가 설치되고 AppData에 생성되는 폴더 구조와 확인 가능한 정보에 대해서 파악합니다.

#### KakaoTalk 폴더 구조
- 경로 : C:\Users\\[User]\AppData\Local\Kakao\KakaoTalk
![kakaotalk_폴더구조.png](/카카오톡/kakaotalk_폴더구조.png)

#### KakaoTalk\Users 폴더 구조
- 경로 : C:\Users\\[User]\AppData\Local\Kakao\KakaoTalk\users
![kakaotalk_users_폴더구조1.png](/카카오톡/kakaotalk_users_폴더구조1.png)
![kakaotalk_users_폴더구조2.png](/카카오톡/kakaotalk_users_폴더구조2.png)
위 사진의 "11ec~36b9" 폴더는 2022-09-27 19:14분에 마지막으로 로그인 했던 A계정 정보, 다음 "611e~d24a" 폴더는 2022-09-27 20:00에 로그인한 B계정 정보를 가지고 있습니다. 확인한 결과 PC에서 각 계정에 로그인할 때마다 수정한 날짜가 변경됩니다.

##### 611e~d24a 폴더 구조
- 611e~d24a 폴더 구조 :
![kakaotalk_users_611e2f6ce6e37e184e24e5bd1a7bc8b8c262d24a.png](/카카오톡/kakaotalk_users_611e2f6ce6e37e184e24e5bd1a7bc8b8c262d24a.png)
- chat_data 폴더 구조 :
![kakaotalk_users_611e2f6ce6e37e184e24e5bd1a7bc8b8c262d24a_chat_data.png](/카카오톡/kakaotalk_users_611e2f6ce6e37e184e24e5bd1a7bc8b8c262d24a_chat_data.png)
특히 chat_data의 chatLogs_\**.edb파일을 이용한다면 삭제된 채팅을 복구할 수 있습니다.
> 참고: chatLogs_\**.edb파일의 개수는 126개고, PC 카톡에서 확인 가능한(위 계정) 채팅방의 수도 126개로 동일했습니다.
{.is-info}


##### Last_pc_login.dat
![kakaotalk_users_lastpclogindat_내용.png](/카카오톡/kakaotalk_users_lastpclogindat_내용.png)
**마지막으로 PC에서 로그인한 계정 정보**, 로그인 시각(로그인 하면 파일 수정 시간이 마찬가지로 변함) 등이 나타나 있습니다.

##### Login_list.dat
![kakaotalk_users_lastlistdat_내용.png](/카카오톡/kakaotalk_users_lastlistdat_내용.png)
Registry에서 확인할 수 있었던 PC에 등록된 계정 리스트를 확인이 가능합니다.

## 마무리
Windows에서 확인 가능한 Kakao.exe 아티팩트인 Registry, %LOCALAPPDATA%\Kakao 경로 파일을 조사했습니다. 각 레지스트리 하위 키, 값 / 파일 세부 분석은 진행하지 않았지만 표면적으로 얻을 수 있는 정보는 "상세 분석"과 같습니다.

의미 있는 정보는 Registry에서 PC 등록 후 자동 로그인을 설정한 계정 ID(e-mail)를 바로 확인이 가능하다는 것이며, Kakao 파일 경로에서 계정 리스트와 최근 로그인한 계정이 확인이 가능하다는 점입니다. 마지막으로 chat_data 폴더의 chatLogs_\**.edb 파일을 열면 채팅기록을 확인할 수 있습니다.

## 추가 관련 참조
- Registry : [Registry](/ko/Artifact/Registry/)

