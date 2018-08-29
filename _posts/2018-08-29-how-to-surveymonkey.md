---
layout: post
title: Survey Monkey API 간단 사용법
category: API
tags: [외부API]
---

## [외부API] 서베이 몽키 설문 생성 및 API 사용 준비 포스팅 입니다. 



서베이 몽키 API를 사용하기 위해서는 아래와 같은 순서로 진행 됩니다. 


1. App 생성하기
2. 설문 생성
3. 설문 결과 확인

----
먼저 App 생성 후 Access Token 확인법입니다. 

Survey Monkey Developer
: https://developer.surveymonkey.com/  

Survey Monkey Developer 사이트에 접속하여 로그인 후, 상단의 'MyApps'로 이동합니다.

![enter image description here](https://lh3.googleusercontent.com/XJp1PoS9QMLyitU5B7wAUncCIubY-wrYOSM159U_K5Q12igKEb8cEzis-i67mAA15j8nFZH29z9I)

여기서  'Add a New App' 을 클릭하여 App을 생성 합니다.
(생성 후 Deploy ! )

![enter image description here](https://lh3.googleusercontent.com/9ZmxQLrPB9Qp2OMZX4M_yvX-N9tg_01H0uh6dtsLZQg1BjXZ1j5SKtZ4GVDCbiecO18zTKvY1PDb)

 App Deploy 후 아래의 Settings를 클릭하면 아래와 같은 정보들을 확인 할 수 있습니다.

![enter image description here](https://lh3.googleusercontent.com/o26KhiJDut8au9uwmtieMBKgD2-s0ptLe4C7U717rvFM4FQYnB_cj8Y5mkCKiskGrlYbqAUGDt98)

  여기서 Access Token은 Survey Monkey API 연동시에 사용 되며
 Scopes 설정으로 API사용 여부를 설정합니다.

 Scope의 초기 값은 Not Request로 설정되어 있기에 필요한 API에 대해 클릭하여 Required로 변경해  줍니다.

--------
앱 생성이 완료 되었으면 설문 생성을 위해 Survey 사이트로 이동합니다. 

Survey Monkey Console
:  https://ko.surveymonkey.com/  

여기에선 설문 생성 및 응답 결과를 확인 할 수 있습니다. 

설문 생성은 상단 메뉴 '대시보드'나 '내 설문조사' 에서 설문 조사 만들기를 클릭합니다. 

![enter image description here](https://lh3.googleusercontent.com/czNLV4tF360uckhIgPAZs3B0gywNTec3R13BRdJOpxx5o8R66qrfyj6sJONxBQR3gWgHzlKgXOVa)

생성 후 수집 (Collector) 을 설정하면 웹 링크나 모바일 등으로 설문 조사를 내보낼 수 있습니다.

--------
Survey Monkey API 요청 상세는 아래 사이트에서 확인 할 수 있습니다.

Survey Monkey API Documentation 
: https://developer.surveymonkey.com/api/v3/

여기에 기술되어 있는 API는 Postman의 Collection으로도 제공됩니다. 
이를 이용하면 Survey Monkey API를 빠르게 이해 할 수 있습니다. 

![enter image description here](https://lh3.googleusercontent.com/_ydb73HnHScoDzTYJnwVAHA1-1yM2YffRbFOhC-kpicKAV0r-TkS-xrWX-XVpbWG4eoOutvIe6X8)

상단 오른쪽에 'Run in Postman'을 클릭하면 설치된 Postman에 자동으로 API가 셋팅 됩니다.

![enter image description here](https://lh3.googleusercontent.com/k5PwXAKnj8MhcIKeOYZNVkYs48nGv4PO5_1o0gkWLZ-ts-8ALMMeuXWOEYf4yI5kWRQ9Z1FIb1qx)

Survey Monkey API 요청시에는 포스팅 앞 부분에 기술 되었던 Apps의 Access Token를 입력하여 요청 합니다. 
( App Token 확인 : https://developer.surveymonkey.com/apps/ )

![enter image description here](https://lh3.googleusercontent.com/SNFsDEHz8rAePrTqOxqXN9195qKWhrhHgJ8PcpjUef5RD9KztGnZPUTCA_Kk73L4T2Q6OZt3MxcG)


