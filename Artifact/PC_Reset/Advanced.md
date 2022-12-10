---
title: Advanced
description: Sysreset_Advanced
published: true
date: 2022-10-28T03:27:45.511Z
tags: setupact.log, resetsession.xml, $sysreset, pushbuttonreset.etl, restoredownlvelalluserstore.log
editor: markdown
dateCreated: 2022-10-14T10:01:47.357Z
---

# $SysReset 아티팩트 분석

- 분석 목적 : 분석 대상 PC에 초기화 흔적이 남아있는지, 남아있다면 어떤 방식으로 초기화를 진행했는지에 대한 여부를 특정하기 위함입니다.

- 분석 환경 : Windows 10 21H2

- Analysis Files : $SysReset 폴더

> 일반적으로 $SysReset 폴더는 Windows PC 복구(초기화)를 한 번 이상 수행한 PC에 한해서 C:\ 경로에 위치합니다.
{.is-info}


- 분석 도구

|도구|버전|URL|
|-|-|-|
|Sublime Text|Build 4126|https://www.sublimetext.com/download|
|FTK Imager|4.7.1.2|https://accessdata.com/product-download/ftk-imager-version-4-5|


- VM

|OS|버전|System Name|
|-|-|-|
|Windows 1 - Pro|10.0.19044 Build 19044.1826|DESKTOP-DR9PJ51|

## 상세분석
$SysReset 폴더 내부에는 다음과 같은 폴더 및 파일들이 존재합니다.

![sysreset2.png](/sysreset2.png)

$SysReset 폴더는 PC 초기화 작업에 사용한 파일과 초기화 작업에 대한 마이그레이션 로그가 저장됩니다. 따라서 로그를 분석해보면 복구 작업과 관련한 정보를 얻을 수 있습니다. 
또한, $SysReset 폴더 하위의 폴더와 파일은 복구 중에 생성되므로 생성 시간은 복구 작업을 수행한 시간이 됩니다. 


### ResetSession.xml
ResetSession.xml 파일은 앞선 그림처럼 $SysReset 폴더에 들어가면 바로 존재하는 파일입니다.

![resetsession.png](/resetsession.png)

xml 파일을 열어보면, 위 그림과 같이 복구 및 초기화를 진행하는 과정에서 기록되는 정보가 담겨있습니다. 

![resetsession3.png](/resetsession3.png)

주로 확인해볼 수 있는 요소는 초기화 시 선택했던 옵션에 대한 데이터이고 그 외로는 TargetVolume과 OldOS, NewOS에 관한 정보가 있습니다.


![resetsession1_re.png](/resetsession1_re.png)

![reset_local.png](/reset_local.png)

Scenario는 Basic에서 설명했던 첫번째 단계에서 선택한 옵션과 연관됩니다.
사용자가 모든 파일 제거 옵션을 선택했을 때 **Reset**이 기록되고, 내 파일 유지 옵션을 선택했을 때 **Refresh**가 기록됩니다.

![resetsession4.png](/resetsession4.png)

![reset_wipe.png](/reset_wipe.png)

WipeData는 Basic에 설명했던 첫번째 단계에서 모든 파일 제거 옵션을 선택하게 되면, 세번째 단계에서 보여지는 모든 드라이브에서 파일을 삭제하시겠습니까? 옵션과 연관됩니다.

사용자가 해당 옵션에 대해 "예"를 선택하게 되면 WipeData가 **True**로 기록되고, "아니요"를 선택하게 되면 **False**로 기록됩니다.

![reset_overwrite_true.png](/reset_overwrite_true.png)

![reset_overwrite_false.png](/reset_overwrite_false.png)

OverwriteSpace는 Basic에 설명했던 첫번째 단계에서 모든 파일 제거 옵션을 선택하게 되면, 세번째 단계에서 보여지는 데이터를 정리하시겠습니까? 옵션과 연관됩니다.

사용자가 해당 옵션에 대해 "예"를 선택하게 되면 OverwriteSpace가 **True**로 기록되고, "아니요"를 선택하게 되면 **False**로 기록됩니다.

![reset_workplace_true.png](/reset_workplace_true.png)

![reset_workplace_false.png](/reset_workplace_false.png)

PreserveWorkplace는 Basic에서 첫번째 단계에서 나뉘어집니다.
내 파일 유지를 선택했을 때는 **True**로 기록됩니다.
모든 파일 제거를 선택했을 때는 사전 설치 앱 자체가 남지 않기 때문에 **False**로 기록됩니다.


### RestoreDownLevelAllUserStore.log

$Sysreset 하위 AppxLog 폴더 내에는 RestoreDownLevelAllUserStore.log 파일이 존재합니다. 이 파일은 Windows 앱 패키지 로그로서 초기화 작업 중에 앱 패키지 관련 행위를 기록합니다.
해당 로그를 통해서 두번째 단계에서 사용자가 복구 혹은 초기화를 위해 새 Windows를 Cloud에서 다운로드 받고 재설치했는지, 로컬에 존재하는 Windows를 통해 재설치를 진행했는지에 대한 여부를 확인할 수 있습니다.

![클라_재설치3.png](/클라_재설치3.png)

![cloud_local_공통.png](/cloud_local_공통.png)

두 옵션 모두 공통적으로 위 그림과 같은 루틴이 수행되고 로그에 기록되는데, Cloud download 옵션을 선택하게 되면 위 로그가 기록되기 전에 추가적인 로그가 기록됩니다.


![클라_재설치.png](/클라_재설치.png)
![erasefilesystem1.png](/erasefilesystem1.png)
![클라_재설치2.png](/클라_재설치2.png)

위 두 그림에서 볼 수 있듯이, Cloud download 옵션을 선택할 시에는 기존에 존재하는 package를 찾아서 remove 하는 로그와 해당 패키지가 존재하는 경로를 제거하는 로그가 추가적으로 기록됩니다.

### setupact.log

setupact.log는 $SysReset 하위 Logs 폴더 내에 존재합니다. 해당 파일에는 복구 및 초기화에 대한 setup 로그가 상세히 기록됩니다. 

> setup 진행 중에 발생하는 오류에 대한 로그가 기록되는 setuperr.log도 존재합니다.
{.is-info}

setupact.log에서는 위에서 ResetSession.xml에 남는 기록보다 좀 더 세부적으로 확인할 수 있습니다. 


![erasefilesystem1.png](/erasefilesystem1.png)

![erasefilesystem2.png](/erasefilesystem2.png)


위 두 그림을 통해 첫번째로 확인해 볼 사항은 **데이터를 정리하시겠습니까?** 옵션입니다. 해당 옵션에 대해서 "예"를 누를경우 EraseFileSystem이라는 Operation이 수행됩니다. 이를 통해 드라이브의 metadata를 정리하고 비할당 영역을 완전 삭제합니다.

위 Operation은 **데이터를 정리하시겠습니까?** 옵션에 대해서만 "예"일때 수행됩니다.

----- 

![formatvolume.png](/formatvolume.png)

![formatvolume2.png](/formatvolume2.png)

위 두 그림을 통해 두번째로 확인할 수 있는 사항은 **모든 드라이브에서 파일을 삭제하시겠습니까?** 옵션입니다. 해당 옵션에 대해서 "예"를 누를경우 FormatVolume이라는 Operation이 수행됩니다. 이를 통해 해당 드라이브의 data를 완전삭제가 아닌 format을 진행합니다.

위 Operation은 **모든 드라이브에서 파일을 삭제하시겠습니까?** 옵션에 대해서만 "예"일때 수행됩니다.

> 추가적으로 두 옵션에 대해서 모두 "예"이거나 "아니요"인 경우에서는 해당 Operation을 찾아볼 수 없었으며 두 경우는 같은 24개의 Operation을 수행했습니다.

- 모두 "예" 혹은 "아니요" 경우에 대한 Operation 작업
![operation.png](/operation.png)

이외에도 많은 초기화 setup 정보가 포함되어 있습니다.


### PushButtonReset.etl

해당 ETL(Event Trace Log) 파일 역시 setupact.log가 위치하는 경로에서 확인할 수 있습니다.
이 파일에서도 초기화시 선택했던 옵션을 포함하여 여러 초기화 작업 로그가 기록됩니다.

MMA(Microsoft Message Analyzer) 도구와 ETLparser 도구를 사용해서 로그를 분석할 수 있습니다.

![mma_push.png](/mma_push.png)

![etlparser.png](/etlparser.png)


## 마무리

결론적으로 $SysReset 폴더 및 하위 폴더, 파일들에 대한 로그 데이터를 분석함으로써 분석 대상 PC에서 PC 사용자가 수행했던 Windows PC 복구 및 초기화에 대한 여러 정보를 획득할 수 있습니다.
