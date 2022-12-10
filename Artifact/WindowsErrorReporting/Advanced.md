---
title: Advanced
description: 
published: true
date: 2022-12-02T10:52:57.709Z
tags: 
editor: markdown
dateCreated: 2022-11-10T04:00:55.392Z
---

# Windows Error Reporting(WER)

Windows Error Reporting에서 WERfault.exe 를 통해 어떻게 오류 보고가 남는지 아티팩트를 분석하고 알아봅니다.

- **분석 환경 : Windows 10 Pro 21H2**

## 상세 분석

### Appcrash

Appcrash는 주로 다음과 같은 상황에서 발생하는 이벤트입니다.

- 바이러스 및 멀웨어 감염

- 높은 시스템 리소스 사용량

- 시스템과 프로그램 간의 비호환성

- 시스템 파일 손상

![appcrash_ftk.png](/wer/appcrash_ftk.png)


### AppHang

AppHang은 "특정 응용프로그램이 응답하지 않습니다"를 출력할 때 발생하는 이벤트입니다.

![apphang.png](/wer/apphang.png)

![apphang_evtx.png](/wer/apphang_evtx.png)

Report.wer과 응용프로그램 이벤트로그의 Event Id 1001에서 다음과 같이 확인하실 수 있습니다.




## 마무리
- 윈도우 오류 보고 관련 아티팩트를 통해 특정 프로그램이나 악성코드로 인한 충돌 여부를 확인하고 충돌 시에 발생 시각과 발생 정보를 확인할 수 있습니다.

## 추가 관련 참조
- Evtx : [Advanced](/ko/Artifact/Evtx/응용프로그램및서비스로그/Advanced/)
