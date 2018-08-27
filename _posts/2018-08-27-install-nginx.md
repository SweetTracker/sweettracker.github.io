---
layout: post
title: Nginx 설치
category: Nginx
tags: [Nginx]
---

## [Nginx] ngnix 설치


<br>

<p class="p1"><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">서비스플랫폼팀의</span><span class="s1" style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);"> AN</span><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">입니다</span><span class="s1"><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">~</span><br /><span style="font-size: 12pt; font-family: &quot;맑은 고딕&quot;, sans-serif; color: rgb(0, 0, 0);">
</span>

<p class="p1"><span style="font-size: 12pt;">- yum </span><span class="s1" style="font-size: 12pt;">으로</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">설치시</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">오래된</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">버전이</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">설치</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">되므로</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">따로</span><span style="font-size: 12pt;"> nginx repository </span><span class="s1" style="font-size: 12pt;">를</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">구성하도록</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">한다</span><span style="font-size: 12pt;">.</span></p>
<p class="p1"><span style="font-size: 12pt;"><br /></span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp;&nbsp; &nbsp; </span><span style="font-size: 12pt;">#vi /etc/yum.repos.d/nginx.repo</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp;&nbsp; &nbsp; </span><span class="s1" style="font-size: 12pt;">아래</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">내용을</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">추가</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">한다</span><span style="font-size: 12pt;">.</span></p>
<p class="p2"><br /></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp;&nbsp; &nbsp; </span><span style="font-size: 12pt;">[nginx]</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp;&nbsp; &nbsp; </span><span style="font-size: 12pt;">name=nginx repo</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp;&nbsp; &nbsp; </span><span style="font-size: 12pt;">baseurl=http://nginx.org/packages/centos/7/$basearch/</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp;&nbsp; &nbsp; </span><span style="font-size:12pt;">gpgcheck=0</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp;&nbsp; &nbsp; </span><span style="font-size: 12pt;">enabled=1</span></p>
<p class="p1"><span style="font-size: 12pt;"><br /></span></p>
<p class="p1"><span style="font-size: 12pt;"><br /></span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp; </span><span style="font-size: 12pt;">- </span><span class="s1" style="font-size: 12pt;">설치</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp;&nbsp; </span><span style="font-size: 12pt;">#yum install -y nginx</span></p>
<p class="p1"><span style="font-size: 12pt;"><br /></span></p>
<p class="p1"><span style="font-size: 12pt;"><br /></span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp; </span><span style="font-size: 12pt;">- nginx </span><span class="s1" style="font-size: 12pt;">설정</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp; &nbsp; </span><span class="s1" style="font-size: 12pt;">설정</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">파일</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">위치</span><span style="font-size: 12pt;"> : vim /etc/nginx/nginx.conf</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp; &nbsp; &nbsp; </span><span style="font-size: 12pt;">worker_connections 1024 </span><span class="s1" style="font-size: 12pt;">를</span><span style="font-size: 12pt;"> auto </span><span class="s1" style="font-size: 12pt;">로</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">변경</span><span style="font-size: 12pt;">. (</span><span class="s1" style="font-size: 12pt;">사용할</span><span style="font-size: 12pt;"> cpu </span><span class="s1" style="font-size: 12pt;">코어</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">개수 )</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp;&nbsp; </span><span class="s1" style="font-size: 12pt;">기본</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">설정</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">파일</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">위치</span><span style="font-size: 12pt;">: /etc/nginx/conf.d/default.conf</span></p>
<p class="p1"><span style="font-size: 12pt;"><br /></span></p>
<p class="p1"><span style="font-size: 12pt;"><br /></span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp;</span><span style="font-size: 12pt;">* nginx.conf </span><span class="s1" style="font-size: 12pt;">파일은</span><span style="font-size: 12pt;"> default.conf </span><span class="s1" style="font-size: 12pt;">파일을</span><span style="font-size: 12pt;"> include </span><span class="s1" style="font-size: 12pt;">하고</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">있다</span><span style="font-size: 12pt;">.</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp; &nbsp; </span><span class="s1" style="font-size: 12pt;">따라서</span><span style="font-size: 12pt;"> default.conf </span><span class="s1" style="font-size: 12pt;">파일의</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">설정</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">값을</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">그대로</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">사용</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">할수있다</span><span style="font-size: 12pt;">.</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp; &nbsp; </span><span class="s1" style="font-size: 12pt;">즉</span><span style="font-size: 12pt;"> conf </span><span class="s1" style="font-size: 12pt;">파일에</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">대한</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">위치만</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">별도로</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">설정</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">해주면</span><span style="font-size: 12pt;"> nginx.conf </span><span class="s1" style="font-size: 12pt;">가</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">알아서</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">설정</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">해준다</span><span style="font-size: 12pt;">.</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp; </span><span style="font-size: 12pt;">- default.conf </span><span class="s1" style="font-size: 12pt;">에서</span><span style="font-size: 12pt;"> listen </span><span class="s1" style="font-size: 12pt;">을</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">변경</span><span style="font-size: 12pt;"> </span><span class="s1" style="font-size: 12pt;">해준다</span><span style="font-size: 12pt;">.</span></p>
<p class="p2"><br /></p>
<p class="p2"><br /></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp; </span><span style="font-size: 12pt;">- nginx </span><span class="s1" style="font-size: 12pt;">시작</span></p>
<p class="p1"><span class="Apple-converted-space" style="font-size: 12pt;">&nbsp; &nbsp; </span><span style="font-size: 12pt;">#systemctl start nginx</span></p>
<p class="p2"><br /></p><style type="text/css"><span style="font-size: 12pt;">
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px Helvetica; color: #454545}
p.p2 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px Helvetica; color: #454545; min-height: 14.0px}
p.p3 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px 'Apple SD Gothic Neo'; color: #454545}
span.s1 {font: 12.0px 'Apple SD Gothic Neo'}
span.s2 {font: 12.0px Helvetica}
</span></style><p><br /></p>