---
title: Registry
description: 
published: true
date: 2022-11-27T14:05:16.697Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:12:58.825Z
---

# Registry

Microsoft Computer Dictionary에서는 레지스트리(Registry)를 다음과 같이 정의합니다.

Windows 98, Windows CE, Windows NT 및 Windows 2000에서 사용되는 중앙 계층 구조 데이터베이스는
하나 이상의 사용자, 애플리케이션 및 하드웨어 디바이스에 대해 시스템을 구성하는 데 필요한 정보를 저장할 때 사용됩니다.

즉, Windows 레지스트리는 서비스, 구성 요소, 각종 프로그램처럼 Windows 전반에 걸쳐 
사용되는 설정 그리고 구성을 포함하는 데이터베이스라고 볼 수 있습니다.

## 아티팩트 학습목표
분석자는 해당 아티팩트를 분석하여 윈도우 설치 정보, 계정 정보, 부팅 시 자동으로 실행되는 응용 프로그램 목록, 장치 정보등 윈도우 시스템 상에서 대부분의 정보를 알 수 있습니다. 이를 통해 윈도우 포렌식 분석의 필수적인 요소들을 확인하고 다른 아티팩트와 연관지어 분석을 용이하게 할 수 있습니다.

## 레지스트리 분석의 필요성

레지스트리는 윈도우 시스템을 분석하는데 있어서 필수 요소입니다.
운영체제 정보, 사용자 계정 정보, 응용프로그램 실행 흔적, 저장매체 사용 흔적, 시스템 정보 등 여러 정보를 담고 있어 디지털 포렌식 과정에서 분석에 용이합니다.

## 구조

레지스트리 구조는 계층적 데이터베이스입니다. 데이터는 트리 형식으로 구성되며, 트리의 각 노드를 키(Key)라고 합니다. 

![reg_구조.png](/reg_구조.png)

위 그림에서 키 중에서 가장 상위 키가 되는 키를 루트키(Root Key)라고 합니다. 
그리고 루트키 하위에 존재하는 노드들은 키라고 불리며, 상대적인 기준에 따라 키 하위에 있는 노드들을 하위 키(Sub Key)라고 부르기도 합니다.

각 키에는 값(Values)과 데이터 유형(Data Type), 그리고 데이터(Data)가 존재합니다.

다음은 좀 더 친숙한 의미의 레지스트리 구조 설명입니다.

-   키(Key): 폴더
-   값(Values): 파일, 키 안에 들어 있는 이름/자료


## Hive file

하이브 파일은 레지스트리 정보를 저장하고 있는 물리적인 파일이며 키 값들이 논리적인 구조로 저장되어 있습니다. 
또한, 활성시스템의 커널에서 하이브 파일을 관리하기 때문에 일반적인 방법으로는 접근이 불가능합니다.

하이브 파일이 존재하는 이유는 레지스트리가 휘발성이기때문에 컴퓨터를 재부팅하면 사라진다는 특징이 있기 때문입니다. 
분석 대상 PC가 Live 상태라면 바로 Memory dump를 수행하여 관련 정보를 확인할 수 있지만, 만약 Live System이 아닌 경우에는 레지스트리의 주요 정보를 따로 저장해놓는 하이브 파일을 찾아 필요한 정보를 확인하는 것이 필요합니다.

따라서, 레지스트리는 윈도우 부팅 시 하이브를 지원하는 파일들로부터 값을 읽어와 구성됩니다. 이 파일들은 사용자가 로그온할때마다 업데이트 됩니다.


![reg_hive.png](/reg_hive.png)

레지스트리의 정보는 DISK에 존재하는 것이 아니라 memory에 존재합니다. 

디지털 포렌식에서 레지스트리 분석을 다룰 때 일반적으로 드라이브 이미지를 다루기 때문에 Hive 파일이 저장되는 위치를 아는 것이 중요합니다.



### Hive Set

다음은 활성시스템의 레지스트리를 구성하는 주요 하이브 파일 목록과 루트키 경로입니다.

|파일|레지스트리 루트키 경로|
|-|-|
|NTUSER.DAT|HKEY_CURRENT_USER|
|SAM|HKEY_LOCAL_MACHINE\SAM|
|SECURITY|HKEY_LOCAL_MACHINE\SECURITY|
|SOFTWARE|HKEY_LOCAL_MACHINE\SOFTWARE|
|SYSTEM|HKEY_LOCAL_MACHINE\SYSTEM|
|DEFAULT|HKEY_USERS\.DEFAULT|


- **HKEY_CLASSES_ROOT** : 파일 연관성과 COM(Component Object Model) 객체 등록 정보

- **HKEY_CURRENT_USER** : 현재 시스템에 로그인된 사용자의 사용자 프로파일 정보

- **HKEY_LOCAL_MACHINE** : 시스템의 하드웨어, 소프트웨어 설정 및 다양한 환경 정보

- **HKEY_USER** : 시스템의 모든 사용자와 그룹에 관한 프로파일 정보

- **HKEY_CURRENT_CONFIG** : 시스템이 시작할 때 사용되는 하드웨어 프로파일 정보

- **HKEY_PERFORMANCE_DATA** : 성능 정보 저장


## .LOG

레지스트리 하이브 파일과 함께 존재하는 .LOG 파일은 레지스트리 트랜잭션 로그 파일입니다. 레지스트리 하이브 파일에 최종적으로 기록되기 전에 레지스트리에 기록되는 데이터를 저장하는 저널 역할을 합니다. 레지스트리 파일에 쓰기를 수행할 때 트랜잭션 로그를 사용함으로써 레지스트리의 안정성을 보장할 수 있습니다.

> 트랜잭션 로그에 대한 자세한 사항은 [github 페이지](https://github.com/msuhanov/regf/blob/master/Windows%20registry%20file%20format%20specification.md#transactional-registry-txr) 를 참고하시면 됩니다.
{.is-info}


## TOOLS - REGA

- **버전 1.5.3.0**

![rega.png](/rega.png)

REGA를 통해 레지스트리 하이브 파일을 열게 되면 다음과 같은 그림을 볼 수 있습니다.

"레지스트리 트리" 필드에서는 레지스트리 구조를 보여줍니다.
"도구 상자" 필드에서는 레지스트리에서 얻을 수 있는 정보를 파싱해서 보여줍니다.
"키 속성" 필드에서는 레지스트리 키와 값이 변경되었을 때 최종기록시각과 하위 키 개수, 값 개수를 보여줍니다.
"출력" 필드에서는 키를 선택했을 때 저장된 값을 보여줍니다.

![rega2.png](/rega2.png)

REGA에서는 폴더 내에 하이브 파일을 넣어두고 폴더를 선택하면 하이브파일을 찾고 분석해줍니다.
그림에서 보이는 파일 외에도 UsrClass.Dat, Amcache.hve 파일도 분석이 가능합니다.
비할당 영역도 옵션을 체크하게 되면 분석이 가능합니다.

## TOOLS - Registry Explorer

- **버전 1.6.0.0**

![registry_explorer.png](/registry_explorer.png)

Registry Explorer를 통해 레지스트리 하이브 파일을 열게 되면 다음과 같은 그림을 볼 수 있습니다.
Registry Explorer에서는 왼쪽 뷰에서 레지스트리 구조를 보여줍니다. 
오른쪽 뷰에서는 선택된 키 속 값과 값에 대한 파싱된 데이터를 보여줍니다. 
오른쪽 하단 뷰에서는 값에 대한 raw 데이터를 hex값으로 보여줍니다.

Registry Explorer는 dirty(modified)한 hive 파일을 clean 파일로 변환해주는 기능을 제공합니다. Dirty한 hive파일이란 수정된 data가 hive에 반영되지 않고 .LOG(Transaction Log)에 저장된 경우를 의미합니다. 따라서, Registry explorer에서는 .LOG 파일을 로드하여서 dirty hive를 clean hive로 만들어줍니다.

- **Example**

![regex_dirty.png](/regex_dirty.png)

사용자가 hive 파일을 로드할 때, 위 그림처럼 dirty hive의 경우 감지됩니다.

![regex_dirty2.png](/regex_dirty2.png)

준비해놓은 .LOG 파일들을 함께 로딩해줍니다.

![regex_dirty3.png](/regex_dirty3.png)

Registry Explorer에서는 updated된 clean hive를 저장하도록 합니다.

![regex_dirty4.png](/regex_dirty4.png)

clean hive를 도구에 로드할건지 물어봅니다.


## TOOLS - RegRipper

![regripper2.png](/regripper2.png)

RegRipper에서는 그림과 같이 하이브 파일을 입력하면 dirty/clean 여부부터 RegRipper에서 제공하는 format으로 파싱해서 결과를 보여줍니다.
