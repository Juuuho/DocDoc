---
title: Advanced
description: 
published: true
date: 2022-12-01T09:16:03.057Z
tags: 
editor: markdown
dateCreated: 2022-11-29T09:36:10.208Z
---

# Chrome

- 분석 목적 : Google 계정 동기화가 진행된 분석 대상 PC의 Chrome 관련 아티팩트를 분석합니다.

- 분석 환경 : Microsoft Windows 10 Pro

- Analysis Files : 

|Analysis Files|Artifacts|
|-|-|
|C:\Users{Profile}\AppData\Local\Google\Chrome\User Data\Default\History|Web Artifacts - Chrome|
|C:\Users{Profile}\AppData\Local\Google\Chrome\User Data\Default\Top Sites|Web Artifacts - Chrome|
|C:\Users{Profile}\AppData\Local\Google\Chrome\User Data\Default\Bookmarks.bak|Web Artifacts - Chrome|
|C:\Users{Profile}\AppData\Local\Google\Chrome\User Data\Profile 1\History|Web Artifacts - Chrome|
|C:\Users{Profile}\AppData\Local\Google\Chrome\User Data\Profile 1\Login Data|Web Artifacts - Chrome|
|C:\Users{Profile}\AppData\Local\Google\Chrome\User Data\Profile 1\Shortcuts|Web Artifacts - Chrome|
|C:\Users{Profile}\AppData\Local\Google\Chrome\User Data\Profile 2\History|Web Artifacts - Chrome|
|C:\Users{Profile}\AppData\Local\Google\Chrome\User Data\Profile 2\Google Profile Picture.png|Web Artifacts - Chrome|
|C:\Users{Profile}\AppData\Local\Google\Chrome\User Data\Profile 2\Google Profile.ico|Web Artifacts - Chrome|


- 분석 도구

|도구|버전|URL|
|-|-|-|
|WinSearchDBAnalyzer|1.0.0.6|http://moaistory.blogspot.com/2018/10/winsearchdbanalyzer.html|
|DCode|v5.5|https://www.digital-detective.net/dcode/|
|Visual Studio Code|	1.72.2	|https://code.visualstudio.com/|


- VM

|OS|버전|System Name|
|-|-|-|
|Windows 10 - Pro|10.0.19044 Build 19044.1826|DESKTOP-DR9PJ51|

## 상세 분석

![chrome_check.png](/chrome/chrome_check.png)

Chrome 브라우저의 기능 중 "Google 계정 동기화" 서비스가 있습니다. 해당 기능을 사용한다면 Chrome 아티팩트의 기본 경로인 "Default" 경로 외에도 "Profile {num}" 폴더가 생성됩니다. num은 분석 대상 PC에서 동기화 계정을 추가할 때마다 증가되며, 동기화 순서대로 폴더가 생성됩니다.

![chrome_accounts.png](/chrome/chrome_accounts.png)

위 그림은 VM 환경의 Chrome에 3개의 계정을 동기화한 상태입니다. 

![chrome_profiles.png](/chrome/chrome_profiles.png)

각 계정마다 독립적인 Profile 폴더가 생성되며, 해당 Profile에 Chrome 관련 기록이 저장됩니다. 동기화를 사용하지 않는다면 기존 Default 경로에 저장됩니다.


![chrome_damien_profile1.png](/chrome/chrome_damien_profile1.png)
"Profile 1"폴더

![chrome_default.png](/chrome/chrome_default.png)
"Default" 폴더


위 그림처럼 Profile 폴더와 Default 폴더는 유사한 구조를 가지고 있습니다. 

![chrome_ftk1.png](/chrome/chrome_ftk1.png)

가상 머신 이미지를 FTK를 이용해 Default, Profile 1, Profile 2 폴더를 추출하여 분석을 시작합니다. 분석 과정에서는 Damien이라는 프로필을 사용하고 있는 Profile 1의 History 파일을 중점적으로 분석합니다. 다음은 실험에 사용한 각 폴더의 사용자입니다.

|폴더 이름|사용자|
|-|
|Default|hyeonho Kim|
|Profile 1|Damien Kang|
|Profile 2|허수창|


## 파일 다운로드

![chrome_filedownload1.png](/chrome/chrome_filedownload1.png)
![chrome_filedownload2.png](/chrome/chrome_filedownload2.png)

위 그림은 Profile 1 폴더의 History 파일을 DB Browser for SQLite 도구를 이용해 확인한 결과입니다. 테이블 명은 "downloads"입니다. 해당 테이블에는 파일의 저장 경로, 파일 다운 시작/끝 시각, 다운 받은 경로, tab URL, 다운 받은 파일의 MIME Type 등을 확인할 수 있습니다. 위 그림에서는 "http://journal.dcs.or.kr/xml/23891/23891.pdf" 경로에서 "C:\Users\ChromeTest\Downloads\23891.pdf" 로컬 경로로 다운받은 것을 확인할 수 있습니다.

![chrome_filedownload3.png](/chrome/chrome_filedownload3.png)

위 그림은 start_time 컬럼의 Timestamp를 시간 정보 디코딩 도구인 DCode를 사용하여 확인한 결과입니다. 확인한 결과 "2022-11-30 18:44:26(UTC+9)"에 파일을 다운받은 것을 확인 가능합니다.

![chrome_filedownload4.png](/chrome/chrome_filedownload4.png)

"downloads" 테이블에서 확인할 수 있는 정보 중 url 정보는 "downloads_url_chains" 테이블에서도 확인이 가능합니다.

History 파일의 downloads 테이블에서 분석 대상 PC의 사용자가 Chrome을 이용해 다운받은 파일의 흔적을 확인했습니다.

## 방문 기록

![chrome_url1.png](/chrome/chrome_url1.png)

위 그림은 "urls" 테이블의 정보입니다. 사용자가 접속한 URL의 기록을 확인할 수 있습니다. 순서대로 url, title, 방문 횟수, 방문시각을 확인할 수 있습니다. 다음은 "Touch VPN" 사이트에 접속한 기록에서 얻을 수 있는 정보입니다.

|url|title|visit_count|last_visit_time|
|-|
|https://touchvpn.net/|Touch VPN|2|13314275313310086(2022-11-30 18:57:04(UTC+9))|

## 검색 기록

![chrome_keyword1.png](/chrome/chrome_keyword1.png)

위 그림은 "keyword_search_terms" 테이블의 정보입니다. 사용자가 Chrome을 이용해 검색한 키워드의 정보가 기록되어 있습니다. term, normalized_term에서 검색 기록을 확인할 수 있습니다.

![chrome_keyword2.png](/chrome/chrome_keyword2.png)

또한 위 그림처럼 "urls" 테이블에서 "- Google 검색" 부분이 title 컬럼에서 확인이 된다면 "- Google 검색" 앞 부분의 문자열을 검색했다는 사실을 알 수 있습니다. "keyword_search_terms" 테이블에서는 검색기록만 확인할 수 있다면 "urls" 테이블에서는 검색어, 검색 시각 등을 확인할 수 있습니다.

### Shortcuts

![chrome_login8.png](/chrome/chrome_login8.png)

Web History에 검색 키워드와 유사하며, 저장될 수 있는 정보의 폭이 더 넓습니다.

## 계정 기록

![chrome_login1.png](/chrome/chrome_login1.png)

Chrome을 이용한 다른 사이트 계정 로그인 관련 정보는 "C:\Users{Profile}\AppData\Local\Google\Chrome\User Data\Profile 1\Login Data" 경로에서 확인할 수 있습니다.

### logins 테이블

![chrome_login5.png](/chrome/chrome_login5.png)

실험을 위해 실제로 VM 환경에서 Chrome으로 CNN 사이트에 damien0088@gmail.com으로 가입 후 로그인하였습니다.

![chrome_login2.png](/chrome/chrome_login2.png)

위 그림은 "Login Data" 파일에서 "logins" 테이블 정보를 확인한 결과입니다. 위 그림의 빨간 박스는 순서대로 계정을 입력한 URL, UserName 타입, UserName 값, 기록이 생성된 시각을 의미합니다.

![chrome_login4.png](/chrome/chrome_login4.png)

위 그림은 계정을 입력한 URL을 확인한 결과입니다. CNN 홈페이지에 접속하여 계정을 입력한 것을 확인할 수 있습니다.

![chrome_login3.png](/chrome/chrome_login3.png)

위 그림은 다른 컬럼의 정보입니다. 순서대로 마지막 사용 시각, 패스워드 수정시각을 의미합니다.

|컬럼|정보|
|-|
|original_url|계정 입력 페이지|
|action_url|계정 입력 페이지|
|username_element|ID 타입|
|username_value|ID 값|
|password_element|패스워드 타입|
|password_value|패스워드 값은 암호화되어 있음|
|date_created|기록 생성 시각|
|blacklisted_by_user|1로 활성화 된다면 해당 웹 사이트에 암호를 저장해서는 안 됨을 의미|
|time_used|저장된 로그인 횟수(특정 웹 사이트에 로그인한 횟수가 아님)|
|date_last_used|마지막 사용 시각|
|date_password_modified|패스워드 마지막 수정 시각|

### stats 테이블

![chrome_login6.png](/chrome/chrome_login6.png)

stats 테이블에는 웹 사이트 도메인, 사용자 이름, 로그인 실패 횟수, 업데이트 시각을 확인할 수 있습니다.

위 그림에서 확인할 수 있는 정보는 "https://www.reddit.com/" 사이트에서 "DATadmin1" 사용자 이름을 사용했고, 로그인 실패 횟수는 1회라는 것을 확인할 수 있습니다. update_time 컬럼 시각은 해당 사이트에 로그인(+실패)한 가장 최근 시각을 의미합니다.

## Profile 폴더 사용자 유추

"Profile {num}" 폴더의 사용자가 누구인지 확인하는 방법입니다. 정확하게 어떤 사용자 이메일이 등록되어 있는지 파일은 못 찾았으나 유추할 수 있는 방법은 존재합니다. 분석 대상 PC의 사용자들을 알고 있다면, 해당 Google 계정에 부여된 프로필을 통해 어느정도 유추가 가능합니다.

![chrome_ico1.png](/chrome/chrome_ico1.png)

"Google Profile Picture.png" 파일입니다. 보통 Google 계정을 생성할 때 사용했던 이름 또는 계정의 처음 글자가 프로필에 포함되는 것을 확인할 수 있습니다.

![chrome_ico2.png](/chrome/chrome_ico2.png)

"Google Profile.ico" 파일입니다. 위 그림 또한 사용자의 이름이 프로필에 포함됩니다.

## 자주 방문한 사이트 기록

![chrome_top10_1png.png](/chrome/chrome_top10_1png.png)

위 그림은 Host PC에서 확인한 정보입니다. "Top Sites" 파일을 열면 사용자가 자주 접근한 상위 10개의 사이트를 확인할 수 있습니다. "url_rank"는 해당 URL의 Top 10 Rank를 의미합니다.

## 북마크 기록

![chrome_bookmarkpng2.png](/chrome/chrome_bookmarkpng2.png)

위 그림은 실제로 Chrome 브라우저로 확인한 북마크 기록입니다. 해당 북마크 기록이 Chrome 폴더에 기록되는지 확인해 봅니다.

![chrome_bookmarkpng.png](/chrome/chrome_bookmarkpng.png)

위 그림은 사용자가 설정한 북마크 기록입니다. "Bookmarks.bak" 파일을 확인하면 북마크 추가 시각, 북마크 사용 시각, 북마크 이름, 북마크 URL 등을 확인할 수 있습니다. 


## 마무리
- 본 페이지에서는 Googe 계정 동기화가 진행된 상태의 PC의 Chrome 관련 아티팩트를 확인하고, 사용자의 행위에 따른 분석을 진행했습니다. Chrome 계정 동기화를 진행하지 않는다면 "Default" 경로에 존재하는 History에 흔적이 기록되지만, 동기화를 진행한다면 "Default", "Profile 1", "Profile 3"...로 파일이 생성되고 기록되는 것을 확인했습니다. 해당 실험에서는 처음 동기화를 진행한 계정의 정보가 "Default" 경로에 기록되면서 해당 경로를 Profile 폴더처럼 사용했습니다(따라서 총 3개의 동기화 = Profile 파일 2개).

## 추가 관련 참조
- foxton Forensics - https://www.foxtonforensics.com/blog/post/analysing-chrome-login-data