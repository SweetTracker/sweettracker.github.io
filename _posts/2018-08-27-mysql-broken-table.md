---
layout: post
title: MySQL 깨진 테이블 복구법
category: MySQL
tags: [MySQL]
---

## [MySQL] 깨진 테이블 복구법 (Table is marked as crashed and should be repaired)


<br>

<p class="p1"><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">서비스플랫폼팀의</span><span class="s1" style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);"> AN</span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">입니다</span><span class="s1"><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">~</span><br /><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">
</span></span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">지난</span><span class="s1" style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);"> </span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">번에</span><span class="s1" style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);"> </span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">업체</span><span class="s1" style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);"> </span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">테이블이</span><span class="s1" style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);"> </span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">깨져서</span><span class="s1" style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);"> </span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">장애가</span><span class="s1" style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);"> </span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">발생했어요</span><span class="s1" style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);"> </span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">ㅜ</span><span class="s1"><br /><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">
</span></span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">그래서</span><span class="s1" style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);"> </span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">여기</span><span class="s1" style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);"> </span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">포스팅을</span><span class="s1" style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">.. </span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">ㅎㅎ</span><span class="s1"><br />
</span></p>


<p><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">MySql에서 테이블이 깨졌을 시 복구 법 입니다.</span></p>
<p><span style="font-size: 12pt;">테이블이 깨지게 되면 DB 테이블이 갑자기 Select 가 안되고 Select 시 아래와 같은 에러가 발생합니다.&nbsp;</span></p>
<p><b><br /></b></p>
<p><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt; background-color: rgb(255, 255, 255); color: rgb(255, 0, 0);"><b>........is marked as crashed and should be repaird.&nbsp;</b></span></p>
<p><br /></p>
<p style="font-size: 12px; font-family: 굴림, 돋움; margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><br /></p>
<p style="font-size: 12px; font-family: 굴림, 돋움; margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">테이블이 깨지는 원인은 보통 다음과 같습니다.&nbsp;</span></p>
<p style="font-size: 12px; font-family: 굴림, 돋움; margin: 0cm 0cm 0pt 2em;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">1. 하드디스크의 용량 부족</span></p>
<p style="font-size: 12px; font-family: 굴림, 돋움; margin: 0cm 0cm 0pt 2em;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">2. 서버의 다운</span></p>
<p style="font-size: 12px; font-family: 굴림, 돋움; margin: 0cm 0cm 0pt 2em;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">3. 하드디스크 오류</span></p>
<p style="font-size: 12px; font-family: 굴림, 돋움; margin: 0cm 0cm 0pt 2em;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">4. 기타 MySQL 자체의 일시적인 오류 등&nbsp;</span></p>
<p style="margin: 0cm 0cm 0pt 2em;"><br /></p>
<p style="margin: 0cm 0cm 0pt 2em;"><br /></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">해결 방안으로는 MySQL&nbsp;콘솔에&nbsp;접속하여 repair작업을 진행해 주면됩니다.</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><br /></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">MySQL서버 계정에 접속 한 뒤 아래와 같이 복구 합니다.&nbsp;</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt; background-color: rgb(255, 255, 255); color: rgb(93, 93, 93);">( 테이블이 깨졌을 경우, MySQL 소켓 접속도 안되는 경우가 있습니다.&nbsp;</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt; background-color: rgb(255, 255, 255); color: rgb(93, 93, 93);">이때는&nbsp;/etc/my.cnf 에 소켓 경로를 확인해주시고 재 설정 해주세요)</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><br /></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">1. <b><span style="color: rgb(0, 34, 102);">check table</span></b> 테이블 명;&nbsp;</span></p>
<p style="margin: 0cm 0cm 0pt 2em;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">: 테이블 상태 점검</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">2. <span style="color: rgb(0, 34, 102);"><b>analyze table</b></span> 테이블 명;</span></p>
<p style="margin: 0cm 0cm 0pt 2em;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">: 테이블 보고서 작성</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">3. <span style="color: rgb(0, 34, 102);"><b>repair table</b></span> 테이블 명;</span></p>
<p style="margin: 0cm 0cm 0pt 2em;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">: 테이블 복원</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">4. <b><span style="color: rgb(0, 34, 102);">check table</span></b> 테이블 명;</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><br /></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><br /></p>
<p style="margin-top: 0cm; margin-bottom: 0pt; margin-right: 0cm;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">이 방법으로 해결되지 않은 경우는 반드시 백업을 해두고 MySQL 서비스를 중시 후 myisamchk 작업을 진행합니다.</span></p>
<p style="margin-top: 0cm; margin-bottom: 0pt; margin-right: 0cm;"><br /></p>
<p style="margin-top: 0cm; margin-bottom: 0pt; margin-right: 0cm;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">1. MySQL 데몬 중지</span></p>
<p style="margin-top: 0cm; margin-bottom: 0pt; margin-right: 0cm;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">2. 테이블 파일이 있는 DB 디렉터리까지 이동</span></p>
<p style="margin-top: 0cm; margin-bottom: 0pt; margin-right: 0cm;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">3. <b><span style="color: rgb(0, 34, 102);">myisamchk -ev</span></b><span style="color: rgb(0, 34, 102);"> </span>테이블 명.MYI</span></p>
<p style="margin: 0cm 0cm 0pt 2em;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">: 테이블 파일 상태 점검</span></p>
<p style="margin-top: 0cm; margin-bottom: 0pt; margin-right: 0cm;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">4. <b><span style="color: rgb(0, 34, 102);">myisamchk -r</span></b>&nbsp;테이블 명.MYI</span></p>
<p style="margin: 0cm 0cm 0pt 2em;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 12pt;">: 테이블 파일 복구</span></p>
<p style="margin: 0cm 0cm 0pt 2em;"><br /></p>
<p style="margin: 0cm 0cm 0pt 2em;"><br /></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt;">※ 옵션 설명</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt;">[-s] : 에러 발생 시 메시지 출력</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt;">[-ev] : 정밀하게 검사하여 메시지 출력</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt;">[-r] : 복구</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt;">[-v] : 자세한 메시지 출력&nbsp;</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt;">[-o] : -r 보다 속도는 떨어지지만 안전하고 -r 으로는 복구하지 못하는 것도 복구 가능</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt;">[-e] : -o 로도 복구하지 못하는 경우 사용하나 안전하지는 않음.</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><br /></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt;">※&nbsp;DB 확장자 파일</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt;">*.MYD : 테이블의 데이터를 저장하는 데이터 파일</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt;">*.MYI : 테이블의 인덱스 정보를 저장하는 인덱스 파일</span></p>
<p style="margin-top: 0cm; margin-right: 0cm; margin-bottom: 0pt;"><span style="font-family: &quot;맑은 고딕&quot;, sans-serif; font-size: 11pt;">*.frm : 테이블의 테이블 구조가 저장되는 테이블 스키마 파일&nbsp;</span></p>
<p style="margin: 0cm 0cm 0pt 2em;"><br /></p>
<p style="margin: 0cm 0cm 0pt 2em;"><br /></p>
<p style="margin: 0cm 0cm 0pt 2em;"><br /></p>