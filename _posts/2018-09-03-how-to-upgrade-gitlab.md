====---
layout: post
title: Retrofit과 RxJava로 연쇄 API 호출 코드 수정하기
category: android
tags: [RxJava, Retrofit]
---

## gitlab upgrade(7.6 -> 10.1) 삽질 경험담

안녕하세요.
저는 스윗트래커에서 COO를 맡고 있는 장영남 입니다.

오늘은 저희 스윗트래커에서 gitlab upgrade를 위해 삽질했던 경험을 공유하고자 합니다.

저희 스윗트래커는 소스코드의 현상관리를 위해 gitlab(http://gitlab.com)을 사용하고 있습니다.

이력을 보니 지난 2014년 12월 처음 도입했네요.
그 전까지는 SVN을 이용했습니다.
물론 아직도 git으로 이전하지 못하고 SVN에 남아있는 프로젝트도 있어 모든 프로젝트를 git으로 이전하는게 올해 하나의 목표가 되고 있습니다.

3년 전 git을 도입할 때는 7.6.1 버전의 gitlab  Community Edition 이 설치되어 있었습니다.

이후 버전업은 당연히 없었죠.

지난 주 저희 개발팀 내부에서 gitlab 신규 기능에 대한 확인 요청이 들어왔습니다.

해당 기능은 subgroup(https://docs.gitlab.com/ce/user/group/subgroups/index.html) 으로 gitlab 프로젝트를 관리할 수 있는 기능 중 하나입니다.

저희는 아래 캡쳐와 같이 9개의 그룹이 만들어져 있었고 
하위에 가장 많은 프로젝트를 보유하고 있는 그룹은 150개 정도의 프로젝트를 보유하고 있었습니다.

![](https://dl.dropbox.com/s/7lnn3grvz4doys5/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202018-09-03%2014.31.46.png?dl=0)

이러다 보니 각 프로젝트의 관리가 어려웠고 특정 프로젝트를 찾기위해 많은 시간을 허비해야 했습니다.

gitlab에서 제공하는 subgroup은 group 하위에 다시 group을 생성하여 프로젝트를 편하게 관리할 수 있다는 장점이 있으나
gitlab 9.0에 신규 추가된 기능으로 저희가 사용하고 있던 7.6.1 버전에서는 상상도 할 수 없는 높은 버전이라 gitlab을 upgrade 하기로 맘 먹었습니다.

### gitlab upgrade

