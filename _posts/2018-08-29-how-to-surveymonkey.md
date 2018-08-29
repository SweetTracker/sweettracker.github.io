---
layout: post
title: Survey Monkey API 간단 사용법
category: API
tags: [SurveyMonkey]
---

## 서베이 몽키 API 사용 포스팅 입니다. 

<body class="stackedit">
  <div class="stackedit__html"><p>서베이 몽키 API 사용 준비 포스팅 입니다.</p>
<p>안녕하세요<br>
서비스 플랫폼팀의 AN 입니다.</p>
<p>서베이 몽키 API연동을 하며 사용 준비 단계에 대한 Instruction이 없어 작성하게 되었습니다.</p>
<p>서베이 몽키 API를 사용하기 위해서는 아래와 같은 순서로 진행 됩니다.</p>
<ol>
<li>App 생성하기</li>
<li>설문 생성</li>
<li>설문 결과 확인</li>
</ol>
<hr>
<p>먼저 App 생성 후 Access Token 확인법입니다.</p>
<dl>
<dt>Survey Monkey Developer</dt>
<dd><a href="https://developer.surveymonkey.com/">https://developer.surveymonkey.com/</a></dd>
</dl>
<p>Survey Monkey Developer 사이트에 접속하여 로그인 후, 상단의 'MyApps’로 이동합니다.</p>
<p><img src="https://lh3.googleusercontent.com/XJp1PoS9QMLyitU5B7wAUncCIubY-wrYOSM159U_K5Q12igKEb8cEzis-i67mAA15j8nFZH29z9I" alt="enter image description here"></p>
<p>여기서  ‘Add a New App’ 을 클릭하여 App을 생성 합니다.<br>
(생성 후 Deploy ! )</p>
<p><img src="https://lh3.googleusercontent.com/9ZmxQLrPB9Qp2OMZX4M_yvX-N9tg_01H0uh6dtsLZQg1BjXZ1j5SKtZ4GVDCbiecO18zTKvY1PDb" alt="enter image description here"></p>
<p>App Deploy 후 아래의 Settings를 클릭하면 아래와 같은 정보들을 확인 할 수 있습니다.</p>
<p><img src="https://lh3.googleusercontent.com/o26KhiJDut8au9uwmtieMBKgD2-s0ptLe4C7U717rvFM4FQYnB_cj8Y5mkCKiskGrlYbqAUGDt98" alt="enter image description here"></p>
<p>여기서 Access Token은 Survey Monkey API 연동시에 사용 되며<br>
Scopes 설정으로 API사용 여부를 설정합니다.<br>
<s>(위 Token 값은 테스트 용으로 사용되지 않습니다)</s></p>
<p>Scope의 초기 값은 Not Request로 설정되어 있기에 필요한 API에 대해 클릭하여 Required로 변경해  줍니다.</p>
<hr>
<p>앱 생성이 완료 되었으면 설문 생성을 위해 Survey 사이트로 이동합니다.</p>
<dl>
<dt>Survey Monkey Console</dt>
<dd><a href="https://ko.surveymonkey.com/">https://ko.surveymonkey.com/</a></dd>
</dl>
<p>여기에선 설문 생성 및 응답 결과를 확인 할 수 있습니다.</p>
<p>설문 생성은 상단 메뉴 '대시보드’나 ‘내 설문조사’ 에서 설문 조사 만들기를 클릭합니다.</p>
<p><img src="https://lh3.googleusercontent.com/czNLV4tF360uckhIgPAZs3B0gywNTec3R13BRdJOpxx5o8R66qrfyj6sJONxBQR3gWgHzlKgXOVa" alt="enter image description here"></p>
<p>생성 후 수집 (Collector) 을 설정하면 웹 링크나 모바일, 이메일 등으로 설문 조사를 내보낼 수 있습니다.</p>
<p>Survey 사이트 관련하여 자세한 사항은 아래 링크에서 확인!</p>
<dl>
<dt>Survey Help</dt>
<dd><a href="https://help.surveymonkey.com/">https://help.surveymonkey.com/</a></dd>
</dl>
<h2 id="br"><br></h2>
<p>Survey Monkey API 관련 내용입니다.<br>
API 요청 상세는 아래 사이트에서 확인 할 수 있습니다.</p>
<dl>
<dt>Survey Monkey API Documentation</dt>
<dd><a href="https://developer.surveymonkey.com/api/v3/">https://developer.surveymonkey.com/api/v3/</a></dd>
</dl>
<p>여기에 기술되어 있는 API는 Postman의 Collection으로도 제공됩니다.<br>
이를 이용하면 Survey Monkey API를 빠르게 이해 할 수 있습니다.</p>
<p><img src="https://lh3.googleusercontent.com/_ydb73HnHScoDzTYJnwVAHA1-1yM2YffRbFOhC-kpicKAV0r-TkS-xrWX-XVpbWG4eoOutvIe6X8" alt="enter image description here"></p>
<p>상단 오른쪽에 'Run in Postman’을 클릭하면 설치된 Postman에 자동으로 API가 셋팅 됩니다.</p>
<p><img src="https://lh3.googleusercontent.com/k5PwXAKnj8MhcIKeOYZNVkYs48nGv4PO5_1o0gkWLZ-ts-8ALMMeuXWOEYf4yI5kWRQ9Z1FIb1qx" alt="enter image description here"></p>
<p>Survey Monkey API 요청시에는 포스팅 앞 부분에 기술 되었던 Apps의 Access Token를 입력하여 요청 합니다.<br>
( App Token 확인 : <a href="https://developer.surveymonkey.com/apps/">https://developer.surveymonkey.com/apps/</a> )</p>
<p><img src="https://lh3.googleusercontent.com/SNFsDEHz8rAePrTqOxqXN9195qKWhrhHgJ8PcpjUef5RD9KztGnZPUTCA_Kk73L4T2Q6OZt3MxcG" alt="enter image description here"></p>
<p>그 외 필요한 API나 사용법에 대해서는 API Documentation 사이트를 확인 해주세요.</p>
<p>사용 준비 단계라 뭔가 두서 없이 써내려 갔네요;<br>
설명이 부족한 부분이나 궁굼하신 점 있으면 댓글로 남겨주세요! :)</p>
</div>
</body>
