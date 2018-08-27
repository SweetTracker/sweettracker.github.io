---
layout: post
title: [MySQL] 깨진 테이블 복구법 (Table is marked as crashed and should be repaired)
category: MySQL
tags: [기술블로그]
---

## [MySQL] 깨진 테이블 복구법 (Table is marked as crashed and should be repaired)
MySql에서 테이블이 깨졌을 시 복구 법 입니다.


서비스플랫폼팀의 AN입니다~
지난 번에 업체 테이블이 깨져서 장애가 발생했어요 ㅜ
그래서 여기 포스팅을.. ㅎㅎ


테이블이 깨지게 되면 DB 테이블이 갑자기 Select 가 안되고 Select 시 아래와 같은 에러가 발생합니다.


........is marked as crashed and should be repaird.




테이블이 깨지는 원인은 보통 다음과 같습니다.

1. 하드디스크의 용량 부족

2. 서버의 다운

3. 하드디스크 오류

4. 기타 MySQL 자체의 일시적인 오류 등





해결 방안으로는 MySQL 콘솔에 접속하여 repair작업을 진행해 주면됩니다.



MySQL서버 계정에 접속 한 뒤 아래와 같이 복구 합니다.

( 테이블이 깨졌을 경우, MySQL 소켓 접속도 안되는 경우가 있습니다.

이때는 /etc/my.cnf 에 소켓 경로를 확인해주시고 재 설정 해주세요)



1. check table 테이블 명;

: 테이블 상태 점검

2. analyze table 테이블 명;

: 테이블 보고서 작성

3. repair table 테이블 명;

: 테이블 복원

4. check table 테이블 명;





이 방법으로 해결되지 않은 경우는 반드시 백업을 해두고 MySQL 서비스를 중시 후 myisamchk 작업을 진행합니다.



1. MySQL 데몬 중지

2. 테이블 파일이 있는 DB 디렉터리까지 이동

3. myisamchk -ev 테이블 명.MYI

: 테이블 파일 상태 점검

4. myisamchk -r 테이블 명.MYI

: 테이블 파일 복구





※ 옵션 설명

[-s] : 에러 발생 시 메시지 출력

[-ev] : 정밀하게 검사하여 메시지 출력

[-r] : 복구

[-v] : 자세한 메시지 출력

[-o] : -r 보다 속도는 떨어지지만 안전하고 -r 으로는 복구하지 못하는 것도 복구 가능

[-e] : -o 로도 복구하지 못하는 경우 사용하나 안전하지는 않음.



※ DB 확장자 파일

*.MYD : 테이블의 데이터를 저장하는 데이터 파일

*.MYI : 테이블의 인덱스 정보를 저장하는 인덱스 파일

*.frm : 테이블의 테이블 구조가 저장되는 테이블 스키마 파일

---
layout: post -> 고정값
title: 제목
category: 카테고리
tags: [태그]
---

```






****