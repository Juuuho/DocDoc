---
title: Advanced
description: 
published: true
date: 2022-11-26T05:09:22.232Z
tags: 
editor: markdown
dateCreated: 2022-10-11T10:55:29.100Z
---

# Amcache
- 분석 목적 : 본 페이지에서는 Basic에서 수집했던 Amcache.hve 파일을 AmcacheParser를 통해 추출하여 분석할 수 있는 정보를 확인합니다.

- 분석 환경 : Microsoft Windows 10 Pro

- Analysis Files : 

|Analysis Files|Artifacts|
|-|-|
|C:\Windows\appcompat\Programs\Amcache.hve|Amcache|

- 분석 도구

|도구|버전|URL|
|-|-|-|
|AmcacheParser|version 1.4.0.0|https://github.com/EricZimmerman/AmcacheParser|


## AmcacheParser
![amcacheparser.png](/amcache/amcacheparser.png)
AmcacheParser가 존재하는 경로를 기억합니다. 프로그램은 위의 사진과 같습니다.

![amcacheparser3.png](/amcache/amcacheparser3.png)
CLI로 동작하는 프로그램입니다. cmd에서 위 사진의 마지막 명령어와 같이 Amcache.hve 존재하는 경로를 입력하고, "--csv"를 이용하여 csv 파일로 추출합니다. 추출 경로는 직접 입력합니다.

![amcacheparser4.png](/amcache/amcacheparser4.png)
실제 실행 명령어입니다. 위 사진의 빨간 박스는 실행이 완료되고, 결과가 저장된 경로를 확인할 수 있습니다.

![amcache_result.png](/amcache/amcache_result.png)
위 사진은 툴을 사용한 결과입니다. 다음 표를 통해 파싱 결과를 확인할 수 있습니다.

|결과|설명|
|-|-|
|DeviceContainers|블루투스, 프린터, 오디오, 저장소 등의 항목을 추적|
|DevicePnps|PC에 연결될 수 있는 USB 장치 PNP(Plug N Play)를 의미|
|DeiveBinaries|바이너리 드라이버 관련 정보|
|DriverPackages|드라이버 패키지 정보|
|ShortCuts|lnk 파일 경로 등 확인 가능|
|UnassociatedFileEntrie|수집은 했으나 깨진파일을 모아둔 곳(실제로 Amacache.hve 해당 없음)|

다음 단락들은 각 결과에서 확인할 수 있는 카테고리 정보를 표로 정리합니다.

### DeviceContainers
|카테고리|설명|
|-|-|
|KeyName|device 구분 ID|
|KeyLastWriteTimestamp|마지막 사용 시간|
|Categories|장치 종류|
|DiscoveryMethod|해당 Device 획득 방법, ex. 블루투스, upnp, wsd 등|
|FriendlyName|식별자|
|Icon|아이콘 경로|
|IsActive|활성화 상태|
|IsConnected|연결 상태|
|IsMachineContainer||
|IsNetworked|네트워크 연결 필요 상태|
|IsPaired|통신 필요 상태|
|Manufacturer|제조업체 명|
|ModelId|장치 고유 ID|
|ModelName|장치 이름|
|ModelNumber|장치 Number|
|PrimaryCategory|-|
|State|-|


### DevicePnps
|카테고리|설명|
|-|-|
|KeyName|device 구분 ID|
|KeyLastWriteTimestamp|마지막 사용 시간|
|BusReportedDescription|Bus 보고 장치 설명|
|Class|드라이버 장치 설정 클래스|
|ClassGuid|드라이버 장치 클래스 GUID|
|Compid|대상 장치 호환 가능 ID|
|ContainerId|시스템 제공 고유 식별자|
|Description|장치 설명|
|DriverId|driver 구분 ID|
|DriverPackageStrongName|부모 디렉터리 이름|
|DriverName|드라이버 파일 이름|
|DriverVerDate|드라이버 날짜|
|DriverVerVersion|드라이버 버전|
|Enumerator|장치 열거 버스 식별 값|
|HWID|장치 하드웨어 ID 목록|
|Inf|INF 파일 이름|
|InstallState|장치 설치 상태|
|Manufacturer|장치 제조사|
|Model|장치 모델|
|ParentId|장치 부모의 ID|
|ProblemCode|오류 코드 반환 값|
|Provider|장치 공급자|
|Service|장치 서비스|
|Stackid|스택 하드웨어 ID|

Device Class에서 확인할 수 있는 내용들은 다음과 같습니다.
- system, processor, bluetooth, net, monitor, media, hidclass, battery, keyboard, mouse, display, hdc, usb, scsiadapter, computer, image, cdrom, diskdrive, volume, softwaredevice, wsdprintdevice, audioendpoint, printqueue, printer, firmware, ComputerHardwareId

### DeiveBinaries
|카테고리|설명|
|-|-|
|KeyName|device 구분 ID|
|KeyLastWriteTimestamp|마지막 사용 시간|
|DriverTimeStamp|드라이버 TimeStamp|
|DriverLastWriteTime|Driver 마지막 사용 시간|
|DriverName|드라이버 파일 이름|
|DriverInBox|OS에 드라이버 포함(True/False)|
|DriverIsKernelMode|커널 모드 드라이버(True/False)|
|DriverSigned|-|
|DriverCheckSum|드라이버 CheckSum 값|
|DriverCompany|드라이버 개발 회사|
|DriverId|드라이버 구분 ID|
|DriverPackageStrongName|드라이버 패키지 강력한 이름|
|DriverType|드라이버 타입|
|DriverVersion|드라이버 파일 버전|
|ImageSize|드라이버 파일 크기|
|Inf|INF 파일 이름|
|Product|드라이버 파일 포함 제품|
|ProductVersion|드라이버 파일 포함 제품 버전|
|Service|장치 서비스|
|WdfVersion|Windows 드라이버 프레임워크 버전|


### ShortCuts
|카테고리|설명|
|-|-|
|KeyName|"파일이름"\|"매칭되는 특정 문자열"|
|LnkName|lnk 파일 경로|
|KeyLastWriteTimestamp|마지막 사용 시간|


### UnassociatedFileEntrie
|카테고리|설명|
|-|-|
|ApplicationName|"Unassociated"로 기록|
|ProgramId|프로그램 구분 ID|
|FileKeyLastWriteTimestamp|마지막 사용 시간|
|SHA1|SHA1 값|
|IsOsComponent|OS 구성요소 확인(True/False)|
|FullPath|파일 경로|
|Name|파일 이름|
|FileExtension|파일 확장자|
|LinkDate|파일 연결 날짜|
|ProductName|제품 이름|
|Size|파일 크기|
|Version|파일 버전|
|ProductVersion|제품 버전|
|LongPathHash|파일 전체 경로 Hash|
|BinaryType|이진 타입|
|IsPeFile|PE 파일 확인(True/False)|
|BinFileVersion|파일버전 8진수로 치환 ex.107.0.5304.107|
|BinProductVersion|제품버전 8진수로 치환 ex. 107.0.5304.107|
|Usn|-|
|Language|프로그램 언어|
|Description|장치 설명|



## 마무리
- Amcache.hve를 분석할 수 있는 도구인 AmcacheParser를 사용하는 방법과 얻을 수 있는 정보를 확인했습니다. Amcache는 Prefetch, Iconcache.db 파일과 함께 분석하여 응용 프로그램 실행 여부를 통해 안티포렌식 기술 사용 여부 등을 확인할 수 있습니다. 또한 응용 프로그램 실행 경로, 삭제시간 등을 파악할 수 있습니다.
## 추가 관련 참조
- Prefetch: [Basic](/ko/Artifact/Prefetch/Basic), [Advanced](/ko/Artifact/Prefetch/Advanced)
- binary foray: https://binaryforay.blogspot.com/2017/10/amcache-still-rules-everything-around.html