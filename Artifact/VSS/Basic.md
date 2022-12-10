---
title: Basic
description: 
published: true
date: 2022-11-27T04:36:09.264Z
tags: 
editor: markdown
dateCreated: 2022-11-12T14:43:51.566Z
---

# Volume Shadow Copy(VSS)

볼륨 쉐도우 카피는 Windows 7, 비스타 이후 부터 적용되는 기술입니다. 과거의 XP 버전에서의 시스템 복원지점 대신 현재는 볼륨 쉐도우 카피로 사용되고 있다고 보시면 됩니다.

시스템 복원 지점은 운영체제 재설치 없이 시스템을 백업한 과거의 특정 시점으로 복원하는 기능으로 운영체제 설치 시 기본으로 활성화되어 있습니다. 

볼륨 쉐도우 복사본(스냅샷 또는 특정 시점 복사본)은 다음과 같은 상황에서 사용될 수 있습니다.

- **다른 하드 디스크 드라이브, 테이프 또는 다른 이동식 미디어에 데이터를 보관하는 것을 포함하여 애플리케이션 데이터 및 시스템 상태 정보 백업**

- **디스크 백업**

- **데이터 손실 복구**


## VSS 작동방식

![vss1.png](/vss/vss1.png)


1. VSS Requestor : 쉐도우 복사본의 실제 생성을 요청하는 소프트웨어입니다. VSS Requestor에는 Windows에서 실행되는 대부분의 백업 소프트웨어가 포함됩니다.

2. VSS Writers : 백업할 일관된 데이터 집합이 있음을 보장하는 구성 요소입니다. SQL이나 Exchange Server같은 대상이 Writer가 될 수 있습니다.

3. VSS provider : 쉐도우 복사본을 만들고 유지 관리하는 구성 요소입니다.


## VSS 생성

![vss3_수정.png](/vss/vss3_수정.png)

![vss2.png](/vss/vss2.png)

볼륨 쉐도우 복사본을 생성하려면 위의 두 그림과 같이 [시스템 속성] - [시스템 보호] GUI에서 **"만들기"**를 통해 생성하거나, CMD에서 CLI로 **"wmic shadowcopy call create volume='C:\\'** 명령어를 입력하면 생성됩니다.

![vss_4.png](/vss/vss_4.png)

볼륨 쉐도우 복사본 관리는 **"vssadmin"** 명령어를 통해 위 그림에서 나열된 명령과 함께 사용됩니다.

![vss5.png](/vss/vss5.png)

위 그림은 **"vssadmin list shadows"** 명령어를 입력한 결과입니다. 생성되어 있는 볼륨 쉐도우 카피본에 대한 정보를 출력합니다.

쉐도우 복사본에 포함된 정보는 **쉐도우 복사본 생성 시각, 쉐도우 복사본 생성 ID, 원본 볼륨 ID(ex: C:\\), 쉐도우 복사본 볼륨 ID, 원본 컴퓨터, 서비스 컴퓨터, 공급자, 형식, 특성**입니다.

![vss_volume.png](/vss/vss_volume.png)

위 그림은 볼륨 쉐도우 복사본의 볼륨 정보를 확인할 수 있는 **"vssadmin list Volumes"** 명령어입니다.

![vss_storage.png](/vss/vss_storage.png)

위 그림은 볼륨 쉐도우 복사본의 저장소를 확인할 수 있는 **"vssadmin list shadowstorage"** 명령어입니다.


## 추가 관련 참조

더 자세한 사항은 [Advanced](/ko/Artifact/VSS/Advanced) 에서 확인하실 수 있습니다.



