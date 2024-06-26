---
title: Camera
description: 
published: true
date: 2022-12-02T04:41:40.982Z
tags: 
editor: markdown
dateCreated: 2022-10-26T11:21:45.884Z
---

# Camera

- 분석 목적 : 사용자의 카메라 허용여부와 사용흔적을 추적하기 위함

- 분석 환경 : Windows 10 Pro 21H2 (OS 빌드 191206.1406)

- VM

|OS|버전|
|-|-|
|Windows 10 Pro|21H2 (OS 빌드 191206.1406)|

## 상세분석
Windows 10에서는 사용자가 카메라 액세스 허용 여부를 단계적으로 선택할 수 있습니다.

`설정 > 개인 정보 > 앱 사용 권한 > 카메라 탭`
사용자의 경우 위의 순서로 카메라의 권한에 대해 크게 3가지로 부여할 수 있습니다.

|번호|허용 범위|
|-|-|
|1|이 장치의 카메라에 대한 액세스 허용|
|2|앱에서 카메라에 액세스하도록 허용|
|2.1|카메라에 액세스할 수 있는 Microsoft Store 앱 선택|
|3|데스크톱 앱에서 카메라를 사용할 수 있도록 허용|

권한에 대한 범위는 1 > 2 > 3 순서대로 권한의 우선순위가 높으며, 상위 권한이 비활성화 될 경우 하위 권한은 자동으로 액세스가 거부됩니다.

```diagram
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIHdpZHRoPSI3MTFweCIgaGVpZ2h0PSIzMjdweCIgdmlld0JveD0iLTAuNSAtMC41IDcxMSAzMjciIGNvbnRlbnQ9IiZsdDtteGZpbGUgaG9zdD0mcXVvdDtlbWJlZC5kaWFncmFtcy5uZXQmcXVvdDsgbW9kaWZpZWQ9JnF1b3Q7MjAyMi0xMC0yNlQxMTo0OTo0MS4zMjBaJnF1b3Q7IGFnZW50PSZxdW90OzUuMCAoV2luZG93cyBOVCAxMC4wOyBXaW42NDsgeDY0KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBDaHJvbWUvMTA2LjAuMC4wIFNhZmFyaS81MzcuMzYmcXVvdDsgZXRhZz0mcXVvdDtjWWpfUGZJS2FYTmJkTmYzQ1pzdCZxdW90OyB2ZXJzaW9uPSZxdW90OzIwLjUuMSZxdW90OyB0eXBlPSZxdW90O2VtYmVkJnF1b3Q7Jmd0OyZsdDtkaWFncmFtIGlkPSZxdW90O1h1M0pGSTF1eTFWbmwwVlhNbXd5JnF1b3Q7IG5hbWU9JnF1b3Q77Y6Y7J207KeALTEmcXVvdDsmZ3Q7NVZoTmo1c3dFUDAxUHU0S1l5QndCSkpzcGFxOTdLRm5MemlBQ3BpQ3MwbjY2enZHZGdLQnJIWlhSR3JWWEdMZXpQamplZDRNQ1NKeGRYeHFhWk4vNHlrcmtXMmxSMFRXeUxheFJYejRrc2hKSWE2UEZaQzFSYXFkTHNCejhadVpTSTN1aTVSMUkwZkJlU21LWmd3bXZLNVpJa1lZYlZ0K0dMdnRlRGxldGFFWm13RFBDUzJuNkk4aUZibENmWHQxd2Ird0lzdk55dGdMbEtXaXhsbWZwTXRweWc4RGlHd1FpVnZPaFJwVng1aVZranpEaTRyYjNyQ2VOOWF5V3J3bndGWUJyN1RjNjdQcGZZbVRPV3pMOTNYS3BMK0ZTSFRJQzhHZUc1cEk2d0d1RjdCY1ZDVThZUmp1aXJLTWVjbmJQcGFzUE0veUtlQ2RhUGxQTnJBNHhGNjdLeG5CYXpIQWQvMEg4T2xKOU9GZVdTdlljUURwa3oweFhqSFJuc0JGVzEzTitzbGNneWI5Y0xtemxjSHl3WDBSY3p0VTUwbDJudnBDSlF3MG0vUE1rdnN5NnprcnozK0x2d25qQkR2RWRaWmhOcmhpMXAweTY1RVpabTFuQVdhZE96T2IraXZQK1JDem9lc1F2QXl6MkhIRzFBWlRhbDFuaGxyc0xVQ3RPME90VjhJS1VRT0RUUFJuVkVEWDBIcEV1dmRyTDZ0V1ZCWTFlekE3QzhFRnkweHdWYWoydVo1TVV0M1hhOFBweGRId1BvQ3N6blFFZytHeGViS25rWG1uRXVHQ2ZlZUNBLzVNNnc2K3Z2S1d5YU1OSWxJcTZJUGM0MFBYSnFQUVhBalpjMExKc0wyVkx0MWp4bmxXTXRvVTNXUENLNENURGx5Mk8xb1ZwYnhWdFZ5a2xvdW15NDJwZVprOWtOcE1UNE9rMkhhYjR6ekJteGdGYXhSQlVsbjllSU5DVnc2aUFBVyt0c0pBV1FFRXowMkVRckQyQXhrYjkyNHJKRFBSa3FBZkl4OEdFT2lpSURZenV5akVjdUE3S09wbjlrTVVxblhYTXR4WHk0VnljbjAyNE9ibCtyeWF4aGxZWmR3RUhtVG1WUlVBeVlteDFNZlNyWG5OcnZTdklWb1dXUTJQQ1FpWkFSNUpBUmZ3QWhCcVExV2txVnhtdHJhTXE4OENSWUVZd1o4N0daNFVCY2NPWmpyWkFqVWh1RmtUN2xFQ1BwUHEvNTZpQitKMFVZUXZBbFA2T1l2cVhZSzhyVDB0VWI4WDdWWmE1VlJZVG5JdldmNlZFcFE3MTc4aStpeTg2dXpiL3JPUVZQSDFxNUU3bGVyc1MrY0NValZsWVVhclJsNXZkODRQQytrenZZcmM3RldRcFpEZTFpaUI0YWRoR1BjRFR5cmxuUENmVVUwa20xOWszSHk3bjlra3Z4SkxhRFFGdWpqM1JlaWEvbDFFOUwrckJYdmUxZHZ1VkMwdTloZFJDenhlZmxmM3RzRy9FMlR6Qnc9PSZsdDsvZGlhZ3JhbSZndDsmbHQ7L214ZmlsZSZndDsiPjxkZWZzPjxzdHlsZSB0eXBlPSJ0ZXh0L2NzcyI+QGltcG9ydCB1cmwoImh0dHBzOi8vZm9udHMuZ29vZ2xlYXBpcy5jb20vY3NzP2ZhbWlseT1Ob3RvK1NhbnMrS29yZWFuIik7JiN4YTs8L3N0eWxlPjwvZGVmcz48Zz48cmVjdCB4PSIwIiB5PSI2IiB3aWR0aD0iNzEwIiBoZWlnaHQ9IjMyMCIgZmlsbD0iIzc2NjA4YSIgc3Ryb2tlPSIjNDMyZDU3IiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iNDAiIHk9IjQ2IiB3aWR0aD0iNjMwIiBoZWlnaHQ9IjI0MCIgZmlsbD0iIzY0NzY4NyIgc3Ryb2tlPSIjMzE0MzU0IiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iODUiIHk9Ijg2IiB3aWR0aD0iNTQwIiBoZWlnaHQ9IjE2MCIgZmlsbD0iIzZkODc2NCIgc3Ryb2tlPSIjM2E1NDMxIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iMjgxIiB5PSI3IiB3aWR0aD0iNDI5IiBoZWlnaHQ9IjMwIiBmaWxsPSJub25lIiBzdHJva2U9Im5vbmUiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogNDI3cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMjJweDsgbWFyZ2luLWxlZnQ6IDI4MnB4OyI+PGRpdiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiByZ2IoMCwgMCwgMCk7ICIgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7Ij48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigwLCAwLCAwKTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgb3ZlcmZsb3ctd3JhcDogbm9ybWFsOyI+PHA+PHNwYW4gc3R5bGU9ImxpbmUtaGVpZ2h0OiAxMzAlOyI+PGZvbnQgZGF0YS1mb250LXNyYz0iaHR0cHM6Ly9mb250cy5nb29nbGVhcGlzLmNvbS9jc3M/ZmFtaWx5PU5vdG8rU2FucytLb3JlYW4iIGZhY2U9Ik5vdG8gU2FucyBLb3JlYW4iIHN0eWxlPSIiIHNpemU9IjEiIGNvbG9yPSIjZmZmZmZmIj48YiBzdHlsZT0iZm9udC1zaXplOiAyNXB4OyI+7J20IOyepey5mOydmCDsubTrqZTrnbzsl5Ag64yA7ZWcIOyVoeyEuOyKpCDtl4jsmqk8L2I+PC9mb250Pjwvc3Bhbj48L3A+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjQ5NiIgeT0iMjYiIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj7snbQg7J6l7LmY7J2YIOy5tOuplOudvOyXkCDrjIDtlZwg7JWh7IS47IqkIO2XiOyaqTwvdGV4dD48L3N3aXRjaD48L2c+PHJlY3QgeD0iMjYwIiB5PSI1MSIgd2lkdGg9IjQxMCIgaGVpZ2h0PSIzMCIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJub25lIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDQwOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDY2cHg7IG1hcmdpbi1sZWZ0OiAyNjFweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogI0ZGRkZGRjsgIiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAyNXB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDI1NSwgMjU1LCAyNTUpOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij48c3BhbiBzdHlsZT0ibGluZS1oZWlnaHQ6IDEzMCU7Ij48Zm9udCBkYXRhLWZvbnQtc3JjPSJodHRwczovL2ZvbnRzLmdvb2dsZWFwaXMuY29tL2Nzcz9mYW1pbHk9Tm90bytTYW5zK0tvcmVhbiIgZmFjZT0iTm90byBTYW5zIEtvcmVhbiIgc3R5bGU9ImZvbnQtc2l6ZTogMjVweDsiPjxiPuyVseyXkOyEnCDsubTrqZTrnbzsl5Ag7JWh7IS47Iqk7ZWY64+E66GdIO2XiOyaqTwvYj48L2ZvbnQ+PC9zcGFuPjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48dGV4dCB4PSI0NjUiIHk9Ijc0IiBmaWxsPSIjRkZGRkZGIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjI1cHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPuyVseyXkOyEnCDsubTrqZTrnbzsl5Ag7JWh7IS47Iqk7ZWY64+E66GdIO2XiOyaqTwvdGV4dD48L3N3aXRjaD48L2c+PHJlY3QgeD0iMTA3IiB5PSI5MSIgd2lkdGg9IjUxOCIgaGVpZ2h0PSIzMCIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJub25lIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDUxNnB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDEwNnB4OyBtYXJnaW4tbGVmdDogMTA4cHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6ICNGRkZGRkY7ICIgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7Ij48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMjVweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigyNTUsIDI1NSwgMjU1KTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgb3ZlcmZsb3ctd3JhcDogbm9ybWFsOyI+PGZvbnQgZmFjZT0iTm90byBTYW5zIEtvcmVhbiIgc2l6ZT0iMSI+PGIgc3R5bGU9ImZvbnQtc2l6ZTogMjNweDsiPuuNsOyKpO2BrO2GsSDslbHsl5DshJwg7Lm066mU652866W8IOyCrOyaqe2VoCDsiJgg7J6I64+E66GdIO2XiOyaqTwvYj48L2ZvbnQ+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjM2NiIgeT0iMTE0IiBmaWxsPSIjRkZGRkZGIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjI1cHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPuuNsOyKpO2BrO2GsSDslbHsl5DshJwg7Lm066mU652866W8IOyCrOyaqe2VoCDsiJgg7J6I64+E66GdIO2XiOyaqTwvdGV4dD48L3N3aXRjaD48L2c+PC9nPjxzd2l0Y2g+PGcgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5Ii8+PGEgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMCwtNSkiIHhsaW5rOmhyZWY9Imh0dHBzOi8vd3d3LmRpYWdyYW1zLm5ldC9kb2MvZmFxL3N2Zy1leHBvcnQtdGV4dC1wcm9ibGVtcyIgdGFyZ2V0PSJfYmxhbmsiPjx0ZXh0IHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTBweCIgeD0iNTAlIiB5PSIxMDAlIj5UZXh0IGlzIG5vdCBTVkcgLSBjYW5ub3QgZGlzcGxheTwvdGV4dD48L2E+PC9zd2l0Y2g+PC9zdmc+
```

### 1. 이 장치의 카메라에 대한 액세스 허용
![camera1.png](/application/camera1.png){.align-center}
![camera3.png](/application/camera/camera3.png)
**HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\webcam** 레지스트리에서 허용 여부를 확인할 수 있으며, 허용 시 **Allow** 거부 시 **Deny**로 표시됩니다.

### 2. 앱에서 카메라에 액세스하도록 허용
![camera4.png](/application/camera/camera4.png){.align-center}
![camera5.png](/application/camera/camera5.png)
**HKU\\{USERSID}\SOFTWARE\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\webcam** 레지스트리에서 허용 여부를 확인할 수 있으며, 허용 시 **Allow** 거부 시 **Deny**로 표시됩니다.

#### 2.1 카메라에 액세스할 수 있는 Microsoft Store 앱 선택
![camera6.png](/application/camera/camera6.png){.align-center}
![camera12.png](/application/camera/camera12.png)
Microsoft Store에서 받은 애플리케이션들에 해당하며 개별 앱마다 허용 여부가 레지스트리 서브키로 존재합니다.
**HKU\\{USERSID}\SOFTWARE\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\webcam\Microsoft.{앱이름}\_8wekyb3d8bbwe** 레지스트리에서 허용 여부를 확인할 수 있으며, 허용 시 **Allow** 거부 시 **Deny**로 표시됩니다.
또한, 각 앱이 카메라를 사용한 경우, 마지막으로 사용을 시작한 시각과 종료한 시각이 기록되어 있습니다.


### 3. 데스크톱 앱에서 카메라를 사용할 수 있도록 허용
![camera8.png](/application/camera/camera8.png){.align-center}
![camera9.png](/application/camera/camera9.png)
**HKEY_USERS\\{SID}\SOFTWARE\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\webcam\NonPackaged** 
MS Store에서 다운로드 받은 프로그램을 제외한 프로그램에서 카메라 액세스 허용 여부를 확인할 수 있는 레지스트리입니다.
1, 2번과 달리 개별 앱에 대해 허용 여부를 설정하는것은 불가하며 전체 차단 및 허용만 가능합니다.

![camera11.png](/application/camera/camera11.png)
**HKEY_USERS\\{SID}\SOFTWARE\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\webcam\NonPackaged\\{프로그램 실행 경로}** 
또한, NonPackaged의 서브키로 존재하는 실행 프로그램 경로에서는 각 앱이 마지막으로 카메라를 사용했을때, 사용을 시작한 시각과 종료한 시각이 기록되어 있습니다.


## 마무리
1. 카메라의 액세스 권한은 크게 세가지로 나눌 수 있으며, 각 권한의 여부는 위에서 나열한 레지스트리에서 확인할 수 있습니다.
2. 레지스트리에서는 MS 앱과 MS 이외의 앱이 구분지어있으며, MS 앱은 개별허용이 존재하나 MS 이외의 앱은 개별허용이 존재하지 않습니다.
3. 각 레지스트리 데이터를 보면 해당 앱에서 가장 최근에 카메라를 사용한 시작시간과 종료시간을 알 수 있습니다.
## 추가 관련 참조
- Registry: [Registry](/ko/Artifact/Registry)