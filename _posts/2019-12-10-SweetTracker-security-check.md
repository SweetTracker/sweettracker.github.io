---
layout: post
title: 스윗트래커 SSL 인증서 교체 및 보안점검
category: Web 
tags: [ssl, security, certificate]
---



## 스윗트래커 웹서비스들의 SSL 인증서 교체 작업 및 취약점 점검



안녕하세요. 스윗트래커 JT입니다.

제가 기술블로그에는 첫 포스팅을 하게 되었습니다. 
원래 저희 스윗트래커에서 사용하고 있던 서비스들에 대해 설명하는 포스팅을 작성하고 있었으나 준비가 부족해 아직도 작성중이네요..

이것도 조만간 포스팅할 수 있도록 하겠습니다.

오늘은 어제 작업했던 부분에 대한 기록을 남기고자 기술블로그 포스팅을 작성하게 되었습니다.

모든 웹서비스는 https 통신을 권장하고 있고, 이를 위해서는 SSL 인증서를 발급받아야 합니다.
이 인증서는 상용으로 짧게는 1년, 길게는 최대 3년의 유효기간을 가지며 유효기간이 만료되면 SSL 인증서는 폐기 되어야 합니다.

웹서비스를 이용하다 가끔 아래 화면과 같이 인증서가 만료되었다는 경고를 보셨던 분들도 계실겁니다.

  
![](/assets/images/20191210/bad_ssl.png)
<img src="">

최근 브라우저에서는 개인정보 보호를 위해 해당 SSL 문제에 대해 강력히 경고하고 있습니다.

12월 17일이 저희가 사용하고 있는 api 주소인 sweettracker.net 도메인의 SSL 인증서가 만료되는 날입니다.
이 때문에 만료일 이전에 신규 인증서로 갱신을 해야 했습니다.

SSL인증서를 발급받고, 적용하는건 이미 많은 포스팅이 있기 때문에 
저는 오늘 그 뒷 이야기를 하려고 합니다.

SSL인증서를 적용하게 되면 맞게 적용되었는지 점검을 해야합니다.

https://www.digicert.com/help/<br/>
https://www.ssllabs.com/ssltest/

위와 같은 사이트에서 SSL인증서 적용이 잘 되었는지 확인해볼 수 있습니다.

신규 SSL 인증서를 적용하고 해당 사트를 통해 점검을 진행했는데..

![](/assets/images/20191210/75272140_521089011811711_8603764401267808264_n.jpg)

![](/assets/images/20191210/f_grade.png)

아니.. F등급이라뇨!!!

보안에 자신있던 저희였는데
이 등급을 보고 참을 수 없었습니다.

폭풍 검색을 통해 저 사이트에서 경고하는 부분들을 하나씩 정리하기로 합니다.

# 
This server is vulnerable to the OpenSSL Padding Oracle vulnerability (CVE-2016-2107) and insecure. Grade set to F.

OpenSSL Padding Oracle vuln.
(CVE-2016-2107)	Yes  INSECURE (more info)
https://www.enteroa.com/tag/cve-2016-2107/


#
This server is vulnerable to the POODLE attack. If possible, disable SSL 3 to mitigate. Grade capped to C.

https://www.krcert.or.kr/data/trendView.do?bulletin_writing_sequence=22128<br/>
https://blog.qualys.com/ssllabs/2014/10/15/ssl-3-is-dead-killed-by-the-poodle-attack

SSL 보안 취약점으로 지원하는 TLS보안속성 중 SSL3를 지원하지 않도록 설정하면 보완할 수 있습니다.

#
This server accepts RC4 cipher, but only with older protocols. Grade capped to B.
https://www.securesign.kr/guides/kb/16

This server does not support Forward Secrecy with the reference browsers. Grade capped to B.

This server supports TLS 1.0. Grade will be capped to B from January 2020.

2번과 3번의 항목들은 제공하는 보안속성의 설정을 변경하면 모두 보완이 가능한 
보안취약점 들입니다.

이를 위해 https://mozilla.github.io/server-side-tls/ssl-config-generator/

모질라에서 제공하는 각 웹서비스별 ssl 설정 예시를 참고하면 보안 수준이 높은 안전한 사이트를 서비스 할 수 있습니다.

모든 것을 적용한 이후 확인해보면 A등급이 된 것을 확인할 수 있습니다.

![](/assets/images/20191210/a_grade.png)


해당 점검에서 보여주는 등급이 절대적인 수치는 아닙니다.

다만 보안수준이 낮은 서비스를 제공한다면 해당 보안취약점을 이용 서비스의 위협이 될 수 있기 때문에 
보안취약점에 대해 항상 점검하고 보완하는 정책이 필요할 듯 합니다.

이 포스팅을 보시는 모든 분들이 제공하는 서비스에서는 보안취약점 때문에 피해를 보시는 일이 없었으면 합니다. 

긴 글 읽어주셔서 감사합니다. 
