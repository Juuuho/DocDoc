---
title: Advanced
description: 
published: true
date: 2022-11-25T04:16:51.018Z
tags: 
editor: markdown
dateCreated: 2022-10-11T11:05:22.692Z
---

# Outlook

- 분석 목적 : 분석 대상 PC에서 확인할 수 있는 Outlook 관련 파일들을 통해 메일의 메타데이터, 메일 내용 등을 분석하는 것이 목적입니다.

- 분석 환경 : Microsoft Windows 10 Pro

- Analysis Files : 

|Analysis Files|Artifacts|
|-|-|
|{ID}@outlook.com.ost|ost|
|*.pst|pst|
|*.msg|msg|


- 분석 도구

|도구|버전|URL|
|-|-|-|
|SysTools OST Recovery|8.2|https://www.systoolsgroup.com/outlook-recovery.html|
|View Headers|-|outlook|
|Microsoft Outlook Recovery tool|v.4.8.19.91 64-bit(데모버전)|https://outlook.recoverytoolbox.com/|

- VM

|OS|버전|System Name|
|-|-|-|
|Windows 10 - Pro|10.0.19044 Build 19044.1826|DESKTOP-DR9PJ51|

## 상세분석
Outlook은 MS에서 제공하는 전자메일 서비스입니다.
본 페이지에서 분석하고자 하는 Outlook 관련 파일은 이메일 분석과 맞닿아 있습니다. 관련 파일인 ost, pst, msg 파일을 도구를 통해 확인할 수 있는 정보들을 파악해 봅니다.


### ost
![chris_ost.png](/outlook/chris_ost.png)
분석 대상 ost 파일입니다. ost 파일은 Off-line Outlook 데이터 파일입니다. ost 파일은 계정의 유형이 MS Exchange Server 계정이거나 Windows Live Hotmail 계정일 시에 생성됩니다.

MS 공식홈페이지에 나와있는 내용은 다음과 같습니다.

|유형|설명|데이터 파일|
|-|-|-|
|Microsoft Exchange|Exchange Server 계정|	데이터가 대개 Exchange 메일 서버에 저장됩니다. 컴퓨터에 있는 오프라인 Outlook 데이터 파일(.ost)은 이동하거나 백업할 필요가 없습니다. 이 파일은 새 컴퓨터에서 Outlook에 계정을 추가할 때 다시 만들어집니다.|
|POP 또는 POP/SMTP|POP3|	데이터가 컴퓨터의 Outlook 데이터 파일(.pst)에 저장됩니다. 데이터 항목에 계속 액세스하려면 이 파일을 새 컴퓨터로 이동해야 합니다.|
|MAPI|Windows Live Hotmail(Outlook Hotmail Connector 사용)|데이터가 Windows Live Hotmail 서버에 저장됩니다. 컴퓨터에 있는 오프라인 Outlook 데이터 파일(.ost) 또는 Outlook 데이터 파일(.pst)은 이동하거나 백업할 필요가 없습니다. 이 파일은 새 컴퓨터에서 Outlook에 계정을 추가할 때 다시 만들어집니다.|
|IMAP 또는 IMAP/SMTP|IMAP4 계정|데이터가 메일 서버에 저장됩니다. 또한 동기화된 항목 복사본이 컴퓨터의 Outlook 데이터 파일(.pst)에 저장됩니다. 이 파일은 이동하거나 백업할 필요가 없습니다. 이 파일은 새 컴퓨터에서 Outlook에 계정을 추가할 때 다시 만들어집니다.|

Microsoft Exchange 유형의 설명에서 확인할 수 있듯이 .ost 파일은 백업할 필요 없이 새 컴퓨터에서 Outlook에 계정을 추가할 때 자동으로 생성됩니다.

즉, 분석 대상 PC에서 ost 파일을 의도적으로 삭제하지 않으면 outlook을 이용했던 계정의 ost 파일이 존재합니다.

ost 파일을 분석하기 위해서는 상용도구를 이용해 확인하거나 ost 파일을 pst 파일로 변환 후에 outlook으로 열어 확인해야 합니다.

#### RecoveryToolboxForOutlook
다음은 RecoveryToolboxForOutlook 프로그램을 이용해 ost 파일을 pst파일로 변환하고 outlook으로 열어 확인한 결과입니다.

![ost_to_pst1.png](/outlook/ost_to_pst1.png)
변환하고자 하는 ost 파일을 추가합니다.

![ost_to_pst2.png](/outlook/ost_to_pst2.png)
ost 파일을 pst로 변환하는 옵션을 선택합니다.

![ost_to_pst3.png](/outlook/ost_to_pst3.png)
pst로 변환된 내용을 확인합니다.

![ost_to_pst4.png](/outlook/ost_to_pst4.png)
pst 파일로 저장합니다.

![ost_to_pst5.png](/outlook/ost_to_pst5.png)
기존에 존재했던 ost 파일에서 pst 파일로 변환된 것을 확인할 수 있습니다.
해당 파일은 outlook에서 확인할 수 있습니다.


#### SysTools OST Recovery
다음은 SysTools OST Recovery 프로그램을 이용해서 ost 파일을 pst로 변환과 동시에 메일 내용을 분석하는 과정입니다.

![sys1.png](/outlook/sys1.png)

[File] - [Add File] - [ost 파일 추가] - [Add] 과정을 진행하면 ost 파일을 툴을 통해 확인할 수 있습니다.

위 사진에서 확인할 수 있듯이 보관함(Archive)에서 확인 가능한 메일을 열어보면 Path, From, To, Cc, Bcc, Subject, Attachment, 본문 내용 등을 확인할 수 있습니다. 또한	Hex, Properties, Message Header, MIME, HTML, RTF, Attachments 탭을 이용하여 Message 내용을 분석할 수 있습니다. 

Outlook에는 다양한 기능이 있습니다. 일정, 연락처, 캘린더, 메모 등의 내용이 있으므로 필요하다면 분석 대상 PC의 Outlook 계정 이용자가 사용했던 기능을 추가적으로 분석할 수 있습니다.


### pst
pst 파일은 Outlook 데이터 파일입니다. pst 파일의 경우 ost 분석에서 사용했던 표에서 확인할 수 있듯이 POP3나 SMTP일 경우, IMAP4/SMTP 계정일 경우 생성됩니다.

![pst내보내기.png](/outlook/pst내보내기.png)
또한 Outlook에서 위와 같은 과정으로 현재 메일의 내용 백업을 위해 [Outlook 데이터 파일 내보내기]를 사용하여 pst 파일을 생성할 수 있습니다.

![pst내보내기암호.png](/outlook/pst내보내기암호.png)
내보내기 과정에서 선택적으로 암호를 추가할 수 있습니다.

ost 파일을 확인하기 위해서는 기본적으로 pst 파일로 변환해서 확인을 진행했어야 했지만, pst 파일은 그 자체로 암호가 걸려있지 않다면 Outlook의 [가져오기]를 통해 확인할 수 있습니다.

### msg
![msg.png](/outlook/msg.png)
msg 파일은 Outlook에서 저장하려는 메시지를 더블 클릭 후 다른 메시지로 저장하기를 한다면 생기는 파일의 확장자입니다.

위와 같은 과정을 진행하고 남은 파일은 msg 파일로 존재하며, outlook 소프트웨어가 존재한다면 손쉽게 확인할 수 있습니다.

보통, msg 파일을 분석 대상 PC에서 Outlook의 보관함(Archive)에 추가하지 않고, 따로 파일을 가지고 있다면 중요한 파일일 가능성이 다른 파일에 비해 높습니다.

### View Headers
![viewheaders1.png](/outlook/viewheaders1.png)

Outlook에서 제공하는 확장 프로그램인 View Headers입니다. ost에서 pst로 변환 후 또는 pst 파일을 Outlook에서 열었다면 해당 기능을 이용하여 메일의 Metadata를 자세히 확인할 수 있습니다.

|카테고리|설명|
|-|-|-|
|Summary|메일의 Subject, Message ID, Creation Time, From, To 등의 Header 정보를 화인이 가능합니다. Original Headers를 통해 더욱더 상세한 내용을 확인이 가능합니다.|
|Received|메일의 시작지점부터 자신의 메일함에 도착하기까지 경로를 확인할 수 있습니다.|
|Antispam|Phishing Confidence Level, Spam Filtering Verdict, IP Filter Verdict 등 Spam 메일 관련 Checking 정보를 확인할 수 있습니다.|
|Other|ARC-Seal, ARC-Message-Signature, DKIM-Signature 등의 정보를 확인할 수 있습니다.|

## 마무리
- Outlook 관련 아티팩트인 ost, pst, msg 파일에 대해서 확인했습니다. 분석 대상 PC에서 해당 파일을 분석한다면 PC의 사용자의 행위를 확인할 수 있거나 어떠한 행동양식을 가지고 있는지 어떤 활동을 했는지 등을 파악할 수 있습니다.
## 추가 관련 참조
- windows.edb: [Advanced](/ko/Artifact/Windows/Advanced)