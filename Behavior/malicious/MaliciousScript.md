---
title: 악성 스크립트 삽입
description: MaliciousScript_악성 스크립트 삽입_Default
published: true
date: 2022-12-15T04:18:50.326Z
tags: 
editor: markdown
dateCreated: 2022-11-26T11:57:31.858Z
---

# 악성 스크립트 삽입
본 페이지에 작성된 악성 스크립트 삽입은 주로 문서 파일에서 이루어지는 내용을 다룹니다.

문서 내 삽입된 악성 스크립트나 악성코드는 주로 이메일로 유포됩니다. 스크립트 삽입 문서 대상은 PDF, MS word, HWP 문서 파일 등이 해당됩니다.

각 문서에 삽입되는 악성 스크립트에는 어떤 것이 있는지 간단히 알아보겠습니다.

## PDF

![pdf구조.png](/malicious/pdf구조.png)

위 그림에서 보시는 pdf 파일 구조는 다음과 같습니다.


|구조|설명|
|-|-|
|Header|PDF 파일 버전에 대한 정보가 포함되어 있습니다.|
|Body|이미지, 글꼴 및 기타 멀티 미디어와 모든 Text Streams를 포함하는 문서의 모든 객체를 포함하고 있습니다. 압축되어 있거나 압축되어있지 않은 텍스트, Streams, 이미지, Javascript도 포함할 수 있습니다.|
|Xref Table(Cross-Reference)|파일에서 각 객체의 위치를 나타내는 목록들이며, 모든 객체의 오프셋과 객체들의 사용 상태를 저장합니다. 또한, 문서가 업데이트 되면 'Xref Table'끝에 추가됩니다.
|Trailer|PDF 파일의 메타데이터를 가진 객체의 정보를 가지고 있습니다. 뷰어가 문서를 읽어야 하는 위치를 찾을 수 있도록 도와주는 역할을 하며, 파일의 마지막 줄에는 **%EOF**로 파일의 끝을 표시합니다.|

![pdf_file_object구조.png](/malicious/pdf_file_object구조.png)

위 그림에서 볼 수 있는 PDF를 구성하는 4가지 요소에 대한 정보는 다음과 같습니다.

|대상|설명
|-|-|
|Object|PDF 문서를 구성하기 위해 여러 작은 데이터 오브젝트로 구성된 모임|
|File Structure|파일 구조는 PDF 파일 내에 오브젝트 들이 어떻게 저장되고, 접근되고, 업데이트 되는지 결정하는 요소|
|Document Structure|문서 구조는 오브젝트들이 어떻게 PDF 문서를 구성하고 배치되는지 설명|
|Content Streams|PDF문서의 외곽이나 그래프 요소들을 묘사하는 일련의 명령 모임|


![pdf_file_object구조.png](/malicious/pdf_file_object구조.png)

대부분의 악성 행위는 위 표에서 보시는 구조 중 **"Body"** 영 역에 은닉되어 활동합니다. "Body"영역에는 **"Boolean", "Integer and Real Numbers", "String", "Names", "Arrays", "Dictionaries" 및 "Streams"** 의 8가지 개체로 구성되어 있습니다. 이 중에서 **"Streams", "Names"** 에서 특정 객체로 수행될 때 악의적인 행위가 대부분 발생합니다.

공격자는 "/"로 시작되는 **Names Object**의 고유한 특성을 활용하여 트리거 이벤트를 수행할 때 필요한 '/AA'나 문서가 열릴 때 수행될 작업을 지정하는 '/OpenAction', 'JavaScript'를 동작시키기 위한 '/JavaScript'등을 수행할 수 있습니다.

**Stream Object**에서는 연속적인 바이트의 모음을 사용하여 길이 제약이 없기도 하고, 공격자가 정상적인 개체를 사용하여 난독화 된 문자열과 Streams 형태로 데이터를 숨길 수 있습니다.

![삽입_예시.png](/malicious/삽입_예시.png)

위 그림에선 Names Object에 /Embedded Files로 악성 doc 파일이 삽입된 것을 확인할 수 있고, JS(JavaScript)에서 첨부파일 실행이 가능한 exportDataObject 메소드를 통해 PDF 내에서 악성파일 실행의 발판을 마련하고 있는 것을 확인할 수 있습니다. 

## Docx

> Docx 파일의 구조는 해당 링크에서 자세히 확인하실 수 있습니다.
Docx 기본 정보 : [Basic](/ko/Artifact/DocumentFile/Docx/Basic)
{.is-info}


Docx 파일은 VBS(Visual Basic Script) 매크로를 통해 주로 악성 스크립트가 삽입됩니다. 본 페이지의 절에선 Follina(CVE-2022-30190)로 명명된 제로데이 취약점을 통해 알아보도록 하겠습니다. 본 실험은 학습 목적을 위해 작성되었습니다.

해당 취약점은 Microsoft Support Diagnostic Tool(MSDT) 프로그램이 URL 프로토콜을 통해 호출될 때 발생할 수 있으며, MSDT를 호출한 프로그램의 권한으로 임의의 코드를 원격으로 실행할 수 있습니다. 해당 취약점의 CVSSv3.1 점수는 7.8/10 에 해당하며, 현재 2022년 6월에 취약점에 대한 보안 패치 업데이트가 공개되었습니다.


![rels.png](/malicious/rels.png)

- **경로 : word/_rels**

docx 파일을 zip파일로 변경하였을 때, 내부에 존재하는 다음과 같은 경로에는 document.xml.rels 파일이 존재합니다.


![rels_내부.png](/malicious/rels_내부.png)

rels 내부에는 위 그림의 빨간 박스를 살펴보시면, XML tag의 Relationship에 oleObject 개체를 삽입하고, Target 속성에는 exploit.html을 실행하기 위해 10.10.10.10/exploit.html가 입력된 것을 확인할 수 있습니다.

![exploit1.png](/malicious/exploit1.png)

해당 파일 내에는 위 그림과 같이 정상적인 html 파일인 것처럼 보이지만,

![exploit2.png](/malicious/exploit2.png)

하단에서는 "AAAAA...." 문자열 아래에는 파워쉘과 자바스크립트로 특정 커맨드가 작성된 것을 확인할 수 있습니다. 
해당 커맨드는 ms-msdt 프로토콜을 사용하는 URL을 msdt.exe 파일을 통해 실행되도록 설정되어 있으며 이로 인해 html이 로딩되면, msdt.exe에 공격자가 지정한 악성코드가 인자로 전달되며 실행됩니다.

![레지스트리.png](/malicious/레지스트리.png)

이 취약점을 이용하는 악성 문서는 일반적인 경우와 다르게 사용자가 문서 매크로 기능을 활성화 하지 않아도 작동이 가능하기 때문에 반드시 주의가 필요합니다. Microsoft에서는 해당 취약점에 대응하기 위해 ms-msdt 관련 레지스트리를 백업 후 삭제하는 임시 방안을 제시했습니다.

임시 방편으로는 MSDT URL 프로토콜을 비활성화 시키기 위해 관리자 권한으로 cmd를 실행해서 "reg export HKEY_CLASSES_ROOT\ms-msdt [백업 파일명]" 명령어를 실행하여 레지스트리를 백업하고, "reg delete HKEY_CLASSES_ROOT\ms-msdt /f" 명령어를 실행하여 레지스트리를 삭제할 수 있습니다. 복구를 하기 위해선 관리자 권한의 cmd를 실행하여 "reg import [백업 파일명]"을 실행하면 됩니다.



## Hwp
작성 예정

## 관련 도구

|도구|버전|URL|
|-|-|-|
|010 editor|11.0.1|www.sweetscape.com|


## 관련 참조
- PDF File Structure : https://blog.forensicresearch.kr/4

- PDF exploit scan : https://nurilab.github.io/2021/05/31/pdf_exploit_scan/

- 문서형 악성코드 행위 탐지 연구 : 
http://www.riss.kr/search/detail/DetailView.do?p_mat_type=be54d9b8bc7cdb09&control_no=07fd43084eda4f0dffe0bdc3ef48d419&outLink=K


