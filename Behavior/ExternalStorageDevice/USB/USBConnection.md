---
title: USB 연결 흔적
description: USB 장치 인식 절차_USB 연결 흔적
published: true
date: 2022-11-26T11:53:04.996Z
tags: 
editor: markdown
dateCreated: 2022-11-26T11:53:02.449Z
---

# USB 장치 인식 절차
1. USB 장치가 Windows 시스템에 연결되면 PnP(Plug-and-Play) 관리자가 알림을 수신하고 장치에게 device descriptor 정보를 요청합니다.

2. USB 장치가 Windows 시스템에 처음 연결되면 PnP 관리자는 해당 장치에 대해 로드할 드라이버를 파악하기 위해 전달받은 정보를 기반으로 적절한 드라이버를 검색합니다.

3. 적절한 드라이버가 존재하지 않을 경우, PnP 관리자는 장치 펌웨어로부터 전달받은 드라이버를 설치하고, 설치가 완료되면 USB 장치를 시스템에 마운트하고 관련 이벤트 로그와 레지스트리에 기록합니다.

> [USB Device Descriptor](https://learn.microsoft.com/en-us/windows-hardware/drivers/usbcon/usb-descriptors)는 Vendor와 Product, Serial Number 등 USB 장치 전체에 대한 정보를 포함하고 있습니다.
{.is-info}
# USB 연결 흔적
![프레젠테이션1.png](/behavior/외부저장장치/usb/usb_connect/프레젠테이션1.png)
플러그 앤 플레이(PNP) 관리자는 USB 장치가 Windows 컴퓨터에 처음 연결될 때 USB 장치에서 정보를 추출합니다. 위 그림에서 볼 수 있듯이 여러 위치에 해당 정보를 기록합니다.

## 이벤트로그
> 이벤트 로그의 경우 USB의 사용 흔적이 누락되어 있는 경우가 있으니 주의해야 합니다.
{.is-warning}

### Microsoft-Windows-Partition%4Diagnostic.evtx
![diagevt.png](/diagevt.png)
**해당 이벤트로그에서는 USB의 연결 및 해제 정보를 알 수 있습니다.**
USB를 연결한 경우와 해제한 경우 모두 **이벤트 ID 1006**으로 동일하지만, 연결에 대한 이벤트 로그의 경우 BytesPerSector, Capacity, Manufacturer, Model, SerialNumber, ParentId(VID, PID) 등 USB에 대한 전반적인 정보를 확인할 수 있습니다.
해제의 경우 이벤트로그의 PartitionCount가 0이고 앞서 확인한 BytePerSector와 Capacity가 0으로 표현됩니다.
### System.evtx
![usb_system.png](/usb_system.png)
System 이벤트로그의 경우 후에 설명될 setupapi.dev.log 파일과 같이 USB를 **최초로 사용하였을때 드라이버 설치 이벤트가 발생**하여 기록됩니다.

### Microsoft-Windows-Storage-ClassPnP%4Operational.evtx
![classpnp.png](/classpnp.png)
해당 이벤트로그에서도 GUID, Vendor, Model, Serial Number 등의 정보를 얻을 수 있습니다.

이외에 아래에 명시된 이벤트로그에서도 USB 관련 흔적을 찾을 수 있습니다.
- Microsoft-Windows-WPD-MTPClassDriver%4Operational.evtx

## SetupAPI 디바이스 설치 로그
SetupAPI.dev.log 로그 파일은 기본적으로 `%SystemRoot%\inf\setupapi.dev.log` 경로에 위치합니다.
Windows Vista 버전부터 생성 및 기록되고 있는 로그파일로, 디바이스 설치와 관련된 전반적인 로그를 담고 있습니다.
![setupapi_dev.png](/setupapi_dev.png)
해당 로그에서는 USB 디바이스의 드라이버 설치 과정을 기록하고 있으며 주요 정보는 아래와 같습니다. 해당 시각에 최초로 연결된 USB 장치의 제조사와 모델 등을 확인할 수 있습니다.

|행위|대상|
|-|-|
|최초 연결 시각|Section start 2022/10/12 15:06:53.798|
|Device Class ID|Disk&Ven_SanDisk&Prod_Cruzer_Blade&Rev_1.00|


## 레지스트리
### HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Enum\USB
![usb_regi.png](/usb_regi.png)
해당 레지스트리에서는 분석 대상 PC에 연결된적이 있는 USB장치들이 기록되어 있으며 각 USB의 VID(VID_0000), PID(PID_0000), Serial Number 등을 확인할 수 있습니다.
### HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Enum\USBSTOR
![usbstor1.png](/usbstor1.png)
USBSTOR 서브키에는 Device Class ID, Instacne ID의 차례로 하위키를 가지고 있습니다.
위의 Device Class ID(Disk&Ven_SanDisk&Prod_Cruzer_Blade&Rev_1.00)를 보고 얻을 수 있는 정보는 다음과 같습니다.

|분류|데이터|
|-|-|
|Vendor Name|SanDisk|
|Product Name|Cruzer_Blade|
|Firmware Revision|1.00|
|Serial Number|20043513901DE8F16AFA|

또한, USB와 USBSTOR 하위키에서는 다음의 시간 정보를 얻을 수 있습니다.
기준: ..\\{Serial Number}\Properties\\{83da6326-97a6-4088-9453-a1923f573b29}\\{Number}\default

|분류|데이터|
|-|-|
|0064|First Installed|
|0065|Installed|
|0066|Last Connected|
|0067|Last Removed|

> 다만, 위의 USB와 USBSTOR 서브키를 비롯한 레지스트리의 경우, 설정된 정책에 의해 PnP 관리자가 수시로 접근하여 수정할 수 있어서 레지스트리의 데이터로 최초 연결시각을 단정짓기는 어렵습니다.
{.is-warning}

### HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Portable Devices\Devices
![devices1.png](/devices1.png)
Devices 서브키에서는 연결한 USB 장치의 Serial Number를 이용하여 FriendlyName을 통해 USB 장치의 **볼륨 레이블을 확인**할 수 있습니다.
### HKEY_LOCAL_MACHINE\SYSTEM\MountedDevices
![m1.png](/m1.png)
![m2.png](/m2.png)
MountedDevices 서브키에서는 USB 장치의 Device Class ID 와 Serial Number를 이용하여, 해당 USB 장치의 **드라이브 문자와 볼륨 GUID를 확인**할 수 있습니다.
- **MountPoints2**

### HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\EMDMgmt
EMDMgmt 서브키에서는 VSN(Volume Serial Number)를 확인할 수 있습니다.
하지만, 시스템 드라이브가 SSD가 아닌 HDD일 경우에만 생성됩니다.
> VSN은 고유한 값이지만 [Volume Serial Number Changer](https://www.codeproject.com/Articles/5825/Changing-volume-s-serial-number) 도구를 통해 수정이 가능하므로 주의 해야합니다.
{.is-warning}

- **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\DeviceClasses\{53f56307-b6bf-11d0-94f2-00a0c91efb8b}**
- **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\DeviceClasses\{53f5630d-b6bf-11d0-94f2-00a0c91efb8b}**


## 추가 관련 참조
- 이벤트로그 : **[응용프로그램및서비스로그](/ko/Artifact/Evtx/응용프로그램및서비스로그)**
- 레지스트리 : **[Registry](/ko/Artifact/Registry)**
- 참고: http://forensic-proof.com/archives/3632