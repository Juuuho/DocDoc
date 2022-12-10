---
title: Advanced
description: 
published: true
date: 2022-11-28T12:32:01.381Z
tags: 
editor: markdown
dateCreated: 2022-11-12T14:44:13.371Z
---

# Volume Shadow Copy Advanced

- **분석 목적** : 볼륨 쉐도우 서비스 및 쉐도우 복사본에 대해 자세히 알아봅니다.

- **분석 환경** : Windows 10 Pro 21H2

## 상세분석

![vss6.png](/vss/vss6.png)

볼륨 쉐도우 복사본의 구성을 심볼릭 링크를 생성하여 확인할 수 있습니다.

- **명령어 : mklink /D {드라이브 및 폴더 경로} {생성할 볼륨 쉐도우 카피의 원본 볼륨 경로}**


![vss7.png](/vss/vss7.png)

생성이 완료되면 위 그림과 같이 볼륨 쉐도우 복사본 내에 존재하는 구성은 쉐도우 복사본을 생성한 시점에서의 C 드라이브 내 존재하는 파일 및 폴더의 구성과 동일합니다.


### 레지스트리 내 복원 제외 항목

레지스트리 내 다음과 같은 경로에는 볼륨 쉐도우 복사본을 통한 복원 및 스냅샷 생성 시 백업에 포함되지 않는 파일과 레지스트리 키가 존재합니다.

- **경로 : HKLM\SYSTEM\ControlSet001\Control\BackupRestore\\**

![vss_filenotbackup.png](/vss/vss_filenotbackup.png)

![vss_filenotsnap.png](/vss/vss_filenotsnap.png)

![vss_keynotbackup.png](/vss/vss_keynotbackup.png)


## 마무리
- 볼륨 쉐도우 카피는 디지털 포렌식 관점에서 파일 삭제와 같은 의심스러운 활동을 조사할 때 복구 및 복원을 통해 유용하게 사용할 수 있습니다.



## 추가 관련 참조

