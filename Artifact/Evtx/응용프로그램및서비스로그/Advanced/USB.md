---
title: USB
description: 
published: true
date: 2022-11-28T16:38:54.449Z
tags: 
editor: markdown
dateCreated: 2022-11-15T04:57:03.561Z
---

# USB 장치 관련 이벤트로그

응용 프로그램 및 서비스 로그에는 Windows 로그에 포함되는 Application, Security, Setup, System, Forwarded Events를 제외한 로그들이 기록됩니다.

응용 프로그램 및 서비스 로그에도 디지털 포렌식을 위해 사용되는 로그들이 많이 존재합니다.

예를 들면, USB 사용 흔적 살펴보기 위해 Partition, Storage-ClassPnP 이벤트로그 등을 확인할 수 있으며, 원격 침입 사용자가 수행한 powershell 명령 사용 흔적을 보기 위해 powershell-Operational, powershell 로그를 확인할 수 있습니다.


## USB 관련 이벤트 로그

USB 관련 이벤트 로그에서는 USB 장치 사용 이력, 장치 정보 등을 확인할 수 있습니다.

### Partition/Operational.evtx

Partition 이벤트로그는 Windows 10에 도입된 새로운 이벤트 로그 중 하나에 해당됩니다. 해당 이벤트로그는 장치가 시스템에 연결되거나 연결 해제될 때 **Event ID 1006**에 해당되는 새 이벤트 레코드를 생성합니다.

event 1006은 일반 뷰에서는 "내부에서 사용됩니다"라는 정보만 표출되기때문에 세부사항에서 XML View로 보는 것이 도움됩니다.

![event1006_1.png](/evtx/event1006_1.png)

가장 첫번째로 알 수 있는 System Data 정보입니다. 여기서는 USB 연결 시 생성되는 시스템 시간 정보를 알 수 있습니다. 

![event1006-usb수정.png](/evtx/event1006-usb수정.png)

그림에서도 볼 수 있듯이 연결된 USB의 제조사와 모델, Serial Number, ParentID 정보를 확인할 수 있습니다.

![event1006-partition수정.png](/evtx/event1006-partition수정.png)

또한, 위 그림처럼 연결된 USB에 대한 파티션 테이블 정보, MBR, VBR 값을 확인할 수도 있습니다.

![vbr.png](/evtx/vbr.png)

VBR값을 HxD로 열어보면 연결된 USB에 대한 파일 시스템 정보를 확인할 수 있습니다. 설명을 위해 사용된 Sandisk USB의 File System은 **FAT32**라는 것을 그림을 통해 알 수 있습니다. 

VBR에서는 파일 시스템 정보 외에도 VSN(Volume Serial Number)를 확인할 수 있습니다. 빨간 박스에 존재하는 offset 0x43~0x46을 Little Endian으로 읽으면 **A275-261F**라는 것을 알 수 있습니다.

![mbr수정.png](/evtx/mbr수정.png)

MBR에서는 파티션 테이블 오프셋 0x5에 위치한 **0xC** 값을 통해 해당 USB가 **FAT32**이라는 것도 위 그림을 통해 예시로 확인할 수 있습니다. 

또한, 0x1B8~0x1BB offset에 존재하는 또 다른 빨간색 박스에서 디스크 서명 정보를 알 수 있습니다. 위 그림에서 디스크 서명 정보는 Little-Endian으로 **4433D329**임을 확인할 수 있습니다. 

## 마무리
- 수사관의 관점에서 장치 자체에 액세스하지 않고 남아있는 이벤트 로그 흔적을 통해 장치의 파일 시스템 타입과 디스크 서명을 확인할 수 있습니다. 


## 추가 관련 참조
- USB 연결 흔적 : [USBConnection](/ko/Behavior/ExternalStorageDevice/USB/USBConnection)
- 참고 : https://df-stream.com/2018/05/partition-diagnostic-event-log-and-usb-device-tracking-p1/

