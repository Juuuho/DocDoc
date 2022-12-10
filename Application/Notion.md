---
title: Notion
description: 
published: true
date: 2022-11-10T10:59:56.545Z
tags: 
editor: markdown
dateCreated: 2022-10-12T02:24:53.144Z
---

# Notion 흔적
Notion은 일정, 업무, 기록, 공부, 프로젝트 등을 효율적으로 생성하고 관리할 수 있으며, 협업도 가능한 All-in-one 협업 툴입니다. Notion 프로그램을 설치하여 Windows OS 상에서 사용할 때 생기는 흔적을 분석합니다.


- 분석 목적 : Windows 환경에서 수집 가능한 Notion 흔적입니다.

- 분석 환경 : Microsoft Windows 10 Pro(build 19042) 64-bit

- Analysis Files : C:\Users\\{UserName}\AppData\Roaming\Notion\notion.db

- 분석 도구:

|도구|버전|URL|
|-|-|-|
|DB Browser for SQLite|3.12.2.0|https://sqlitebrowser.org/|

- VM: -


## 상세분석
Notion.exe(PC버전 for Windows)를 설치하고 로그인한 계정 위주의 활동, 데이터, 다른 사용자 정보 등이 notion.db에 기록됩니다. 로그인한 계정을 Root 사용자로 가정하여 설명하겠습니다.

다음은 notion.db를 DB Browser for SQLite로 확인한 테이블 구조입니다.

![테이블구조.png](/notion/테이블구조.png)

다음 소단락에서는 notion.db에 존재하는 테이블 중 유의미한 데이터를 얻을 수 있는 테이블을 분석한 내용입니다.


### user_root
- 획득 가능 정보:
![user_root_획득가능정보.png](/notion/user_root_획득가능정보.png)

해당 테이블은 Notion.exe 앱에 등록하여 로그인한 계정의 정보를 가지고 있습니다. 주의 깊게 봐야하는 것은 id, meta_user_id입니다. 


![userroot_id_meta.png](/notion/userroot_id_meta.png)

두 값은 서로 같으며 DB 구조상 로그인한 계정의 값(식별 번호)을 가지고 있습니다. notion.db에서 유의미한 정보를 파악하기 위해서는 해당 값들을 인지하고 분석을 진행해야 합니다.

![userroot_metarole.png](/notion/userroot_metarole.png)

또한 meta_role은 editor로 명시되어 있습니다.

user_root 테이블에서 유의미한 정보를 가지고 있는 열과 그 값을 정리한 표입니다.

|카테고리|설명|
|-|-|
|id|root 계정의 DB상 식별 ID를 가지고 있습니다.|
|meta_user_id|root 계정의 DB상 식별 ID를 가지고 있습니다.|
|meta_role|notion에서의 권한을 나타냅니다. "전체 허용", "편집 허용", "댓글 허용", "읽기 허용" 순으로 권한부여가 가능합니다. |


### space
- 흭득 가능 정보:
![space_획득가능정보.png](/notion/space_획득가능정보.png)

노션 프로그램의 Root 계정이 생성했던 워크스페이스 정보가 기록됩니다.

![space_전.png](/notion/space_전.png)

새로운 워크스페이스를 생성하기 전에 확인한 DB입니다.

![space_notion워크스페이스생성창.png](/notion/space_notion워크스페이스생성창.png)

Notion.exe에서 워크스페이스를 생성하는 화면입니다.

![space_후.png](/notion/space_후.png)

새로운 워크스페이스를 2개를 생성하고 확인한 DB입니다.

이렇듯, 생성한 워크스페이스의 정보가 해당 테이블에서 즉시 업데이트됩니다. 또한 새롭게 생성했던 워크스페이스를 삭제하면 즉시 DB에서 워크스페이스 내용이 삭제됩니다.

![space_plantype_subscription.png](/notion/space_plantype_subscription.png)

plan_type, subscription 정보입니다. 해당 정보들은 "개인용", "팀용" 워크스페이스 타입 정보를 가지고 있으며, 각 워크스페이스의 구독 정보입니다.

space 테이블에서 유의미한 정보를 가지고 있는 열과 그 값을 정리한 표입니다.

|카테고리|설명|
|-|-|
|id|워크스페이스의 식별ID입니다.|
|name|워크스페이스의 이름입니다.|
|plan_type|워크스페이스의 용도입니다.|
|subscription|워크스페이스의 구독 정보를 가지고 있습니다.|
|created_time|워크스페이스의 생성 시각입니다.|
|created_by_id|워크스페이스를 생성한 계정 ID 정보입니다.|
|last_edited_time|워크스페이스의 마지막 수정 시각입니다.|
|last_edited_by_id|워크스페이스를 마지막으로 수정한 계정 ID 정보입니다.|


### records
- 획득 가능 정보

![records_획득가능정보.png](/notion/records_획득가능정보.png)

records 테이블은 사용자가 작성한 댓글, 페이지, 페이지에 작성한 text, 첨부 파일 등의 기록이 존재합니다. notion_user에서 유지하고 있는 다른 계정의 ID를 통해서 어떤 유저가 댓글을 자신의 글에 달았는지, 누가 자신의 페이지 권한을 바꿨는지 등 Root계정을 중심으로 모든 흔적들이 timestamp 순으로 작성됩니다.

![records_comment.png](/notion/records_comment.png)

위의 그림은 record_table에서 comment로 필터링 후 확인한 내용입니다. record_value에 댓글(Comment)을 작성했던 사용자, 작성 내용, 권한(role) 등을 확인할 수있습니다. 이외에도 어떤 사용자가 자신의 페이지에 들어왔고, 나갔는지 등의 내용들을 확인할 수 있습니다.

records 테이블에서 유의미한 정보를 가지고 있는 열과 그 값을 정리한 표입니다.

|카테고리|설명|
|-|-|
|record_table|record_table 정보의 유형을 나타냅니다.|
|record_value|record_table의 값입니다. 입력한 text, created_by_table, created time, last edited time 등의 정보가 있습니다.|
|timestamp|레코드 정보의 생성시각입니다.|
|user_id|DB상의 root ID입니다.|
|space_id|Space ID입니다.|

### notion_user
- 획득 가능 정보

![notionuser_획득가능정보.png](/notion/notionuser_획득가능정보.png)

notion_user 테이블은 Root 계정을 중심으로 기록된 참여자들의 정보가 저장되어 있습니다. 각 사용자 이름, 이메일, 아이콘 등의 정보가 저장되어 있습니다. Root 계정이 존재하는 페이지에 참여자, Root 계정을 초대한 계정 등 Root 계정과 접촉한 계정 정보가 저장됩니다.

Notion.exe에서 로그아웃하고 다른 계정으로 로그인하더라도 기존에 있던 정보는 유지되고, 그 위에 새로운 계정이 활동하면서 접했던 참여자들의 정보들이 쌓입니다. 다만, 새로운 Root 계정의 정보가 저장되더라도 meta_user_id가 다르기 때문에 구분이 가능합니다.

notion_user 테이블에서 유의미한 정보를 가지고 있는 열과 그 값을 정리한 표입니다.

|카테고리|설명|
|-|-|
|id|DB상에서 유지되는 각 사용자의 ID 값입니다.|
|email|user의 email이 저장됩니다. email은 notion 가입에 사용한 정보입니다.|
|given_name|한국식 이름 표기에서 "이름"을 의미합니다.|
|family_name|한국식 이름 표기에서 "성"을 의미합니다.|
|profile_photo|notion 계정의 프로필 아이콘이 저장되어 있는 링크가 저장되어 있습니다.|
|meta_role|notion에서의 권한을 나타냅니다.|

### collection
- 획득 가능 정보

![collection_획득가능정보png.png](/notion/collection_획득가능정보png.png)

collection table에서는 노션에 존재하는 페이지의 "페이지 설명"이 작성되어 있습니다. 작성자 ID, 페이지 이름, 작성된 Text 등을 확인할 수 있습니다.

![collection_제목소제목2.png](/notion/collection_제목소제목2.png)

위는 실제로 Notion에 저장되어 있는 "연차&반차" 페이지입니다. 해당 페이지의 "설명"을 추가하면 아래와 같은 정보로 DB에 기록됩니다.

![collection_연차반차3.png](/notion/collection_연차반차3.png)

위는 "설명"에 Text를 입력했을 때 기록되는 DB정보입니다.

collection 테이블에서 유의미한 정보를 가지고 있는 열과 그 값을 정리한 표입니다.

|카테고리|설명|
|-|-|
|name|페이지의 제목입니다.|
|description|페이지의 설명이 저장됩니다. |
|icon|페이지의 아이콘이 저장됩니다.|
|meta_role|notion에서의 권한을 나타냅니다.|

### block
- 획득 가능 정보
![block_획득가능정보.png](/notion/block_획득가능정보.png)

Notion에서 Root가 존재하는 공간에서 일어나는 모든 활동이 기록됩니다. 사용자가 언제 어떤 것을 수정했는지, 사용자가 어떤 값을 입력했는지, 어떤 파일을 어디에 업로드했는지 등 다양한 기록들을 확인이 가능합니다.

![block_text.png](/notion/block_text.png)

Root가 존재하는 페이지에서 있었던 모든 행위가 기록됩니다. created_time, last_edited_time이 존재하기 때문에 언제 값이 입력됐는지, 어떤 사용자가 수정했는지 등을 Timestamp와 연관지어서 확인할 수 있습니다.

block 테이블에서 유의미한 정보를 가지고 있는 열과 그 값을 정리한 표입니다.

|카테고리|설명|
|-|-|
|type|Block type|
|properties|입력 값, 첨부 파일 정보가 저장됩니다.|
|copied_from|copy 출처의 정보가 저장됩니다.|
|created_time|해당 기록이 작성된 시각이 저장됩니다.|
|last_edited_time|해당 기록이 마지막으로 수정된 시각이 저장됩니다.|
|meta_last_access_timestamp|사용자가 해당 기록에 마지막으로 접근한 시각이 저장됩니다.|
|last_edited_by|마지막으로 수정한 사용자가 저장됩니다.|



## 마무리
- 결론
notion.db를 분석하면 Root 계정과 관련된 사용자들의 행위 및 데이터들을 파악할 수 있습니다.


## 추가 관련 참조
- X