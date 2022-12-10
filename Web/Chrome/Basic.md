---
title: Basic
description: 
published: true
date: 2022-12-10T05:12:55.294Z
tags: 
editor: markdown
dateCreated: 2022-11-29T09:35:42.263Z
---

# Chrome
Chrome은 Google이 개발 및 서비스를 전담하는, 전세계 사용율이 63.8%에 달하는 웹 브라우저입니다. 해당 브라우저 관련 아티팩트를 조사함으로써, 사용자의 인터넷 방문 기록과 다운로드 기록, 검색 기록 등 다양한 정보를 확인할 수 있습니다.

## 아티팩트 분석 목표
본 페이지에서는 Chrome 사용 시 Windows OS에 남는 아티팩트를 확인합니다.

## 아티팩트 분석의 필요성
Chrome 브라우저의 점유율에서 확인할 수 있듯이 브라우저 이용자의 대부분이 Chrome을 사용중입니다. 따라서 분석 대상 PC, 사용자가 Chrome을 사용했을 가능성이 높습니다. 이러한 상황에서 Chrome 분석을 진행한다면 범행 계획, 수단 등을 확인할 수 있으며, 사용자의 관심사, 행동양식을 파악할 수 있습니다. 또한 Chrome을 이용한 계정 사용기록도 일부분 확인할 수 있으므로 포렌식 관점에서 유용한 아티팩트라고 볼 수 있습니다.

## 아티팩트 경로 및 설명
디지털 포렌식을 위해 사용하는 Chrome의 주요 정보는 Web History, Cache, Cookie, Download File List입니다. 다음은 Chrome 관련 아티팩트와 경로와 설명입니다. 해당 경로들은 Chrome 버전 및 업데이트, 동기화 여부에 따라 달라질 수 있습니다.


|아티팩트|경로|설명|
|-|-|
|Web History|C:\Users\{Profile}\AppData\Local\Google\Chrome\User Data\Default\History|URL, 접속시간, 접속 수, 다운로드 내역, 검색 키워드 등 확인 가능|
|Cache|C:\Users\{Profile}\AppData\Local\Google\Chrome\User Data\Default\Cache\Cache_Data|data_{num} 파일로 구분되어 저장됨, num이 커지면 속도가 느림|
|Cookie|C:\Users\{Profile}\AppData\Local\Google\Chrome\User Data\Default\Cookies|암호화된 쿠키 값|
|Download File|C:\Users\{Profile}\AppData\Local\Google\Chrome\User Data\Default\History|Web History와 같은 경로에서 확인 가능|
|Typed URL|C:\Users\{Profile}\AppData\Local\Google\Chrome\User Data\Default\shortcuts|Web History에 검색 키워드와 유사하며, 정보의 폭이 더 넓음|
|New Tab|C:\Users\{Profile}\AppData\Local\Google\Chrome\User Data\Default\Top Sites|최근 접근한 상위 10개의 URL 기록|
|Bookmarks|C:\Users\{Profile}\AppData\Local\Google\Chrome\User Data\Default\Bookmarks.bak|사용자가 등록한 북마크 기록|
|Visited Link|C:\Users\{Profile}\AppData\Local\Google\Chrome\User Data\Default\Visited Links|암호화된 DB|
|Preferences|C:\Users\{Profile}\AppData\Local\Google\Chrome\User Data\Default\Preferences|백업, 북마크, 브라우저, 검색 엔진, 다운로드, 확장 기능, 플러그인, 클라우드 프린트, DNS 프리패치 정보 등 기록|

## 구조
![chrome_structure.png](/chrome/chrome_structure.png)

앞서 설명드렸던 Chrome 관련 아티팩트들이 존재하는 폴더의 구조입니다.

## 특징
Chrome은 Chromium 기반으로 제작된 웹 브라우저입니다. Chromium은 구글에서 2008년 9월에 시작한 오픈소스 프로젝트입니다. 크롬, 엣지, 오페라, 네이버 웨일, 비발디, 브레이브, 삼성인터넷 등 다양한 브라우저들이 Chromium으로 개발되었습니다. Chromium을 사용하는 브라우저는 Chrome의 아티팩트 구조와 유사합니다. 따라서 Chrome의 아티팩트 분석을 진행했다면 웨일, 엣지 등 인기있는 웹 브라우저의 분석에 도움이 될 수 있습니다.

추가적으로, Chrome에서 제공하는 "계정 동기화"를 사용한다면 해당 계정의 아티팩트는 기존의 경로와 다른 곳에 저장됩니다. 이는 [Advanced](/ko/Web/Chrome/Advanced)에서 자세히 살펴보겠습니다. 

## 도구
- DB Browser for SQLite Tool
- BrowsingHistoryViewTool(GUI, CLI)
- ChromeCacheView
- ChromeForensics
- BrowsingHistoryView

## 추가 관련 참조
더 자세한 사항은 [Advanced](/ko/Web/Chrome/Advanced) 에서 확인하실 수 있습니다.
- Digital Forensics Wikipedia: http://forensic.korea.ac.kr/DFWIKI/