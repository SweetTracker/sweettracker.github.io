---
layout: post
title: RxBinding with Compound
category: Android
tags: [mobile, android, rx, rxbinding]
---


안녕하세요 스윗트래커 모바일팀 SH입니다.

회원가입과 같은 화면을 만들다보면, 다양한 케이스에 대해서 처리를 해줘야합니다..
아이디 입력유무, 비밀번호 입력 유무, 약관 동의 등 다양한 부분들에 대해서 정상적으로 입력이 되었는지에 대해서 확인이 필요합니다.

예전에는 아이디를 입력하는 에디트텍스트에서 값을 받아 empty 체크를 하고, 마찬가지로 비밀번호도 체크하고 각각의 약관에 대해서도 체크박스가 체크되어있는지를 확인했을 것입니다.

이렇게 되면 코드도 복잡해지고, 다양한 케이스에 대해서 처리를 하다보면 더 복잡해지게끔 되어있습니다.

스마트택배의 약관동의 화면만 봐도 다양한 케이스가 존재하는 것을 알 수 있습니다.

![components](/assets/images/smartparcel_term.png)


스윗트래커의 기획자 JP는 어느날 저에게 이러한 요구를 했습니다.
'전체 동의 버튼을 누르면 다음 화면으로 넘어가는데, 서비스 이용약관과 개인정보 취급방침을 각각 체크를 해도 화면이 다음으로 안넘어간다. 수정해달라.'
이쪽 코드에 대해서 살피다보니 케이스 처리가 똑바로 되어있지 않은 부분들을 발견하였고, MVP 패턴으로 리팩토링 하면서 과감하게 뜯어고쳤습니다.

위의 화면대로 보면 다음 화면으로 넘어갈 수 있는 조건은 총 5개가 있습니다.

1. 전체동의 버튼을 누른다
2. 서비스이용약관 체크박스를 체크하고, 개인정보 취급방침 체크박스를 체크한다.
3. 개인정보 취급방침 체크박스를 체크하고, 서비스이용약관 체크박스를 체크한다.
4. 서비스이용약관 체크박스를 체크하고, 전체동의 버튼을 누른다.
5. 개인정보 취급방침 체크박스를 체크하고, 전체동의 버튼을 누른다.

뭔가 케이스마다 각각의 체크박스의 isChecked 값을 확인해가면서 넘어가는건 굉장히 비효율적인 일입니다.

RxBinding을 사용하면 각각의 컴파운드에 대한 이벤트를 Observable로 반환해줍니다. killkillkill


~~~java
    val privacyCheck: Observable<Boolean> = RxCompoundButton.checkedChanges(binding.chkPrivacy)
    val termsCheck:  Observable<Boolean> = RxCompoundButton.checkedChanges(binding.chkTerms)
~~~

우리는 이 각각의 Observable로부터 데이터를 받은 후에 하나의 결과로 방출하는 오퍼레이터인 `combineLatest`를 사용하고자 합니다.

![components](/assets/images/lk-rx3-combinelatest.png)

그림은 두개의 시퀀스가 combineLatest를 만나 하나의 시퀀스가 되는 부분을 표현하고 있습니다.
지금 우리의 케이스과 동일하다고 보면 됩니다.

~~~java
    Observable.combineLatest(termsCheck, privacyCheck) { terms, privacy ->
                    if(terms){
                        binding.chkTerms.setBackgroundResource(R.drawable.ic_check_circle_checked)
                    }else{
                        binding.chkTerms.setBackgroundResource(R.drawable.ic_check_circle)

                    }
                    if(privacy){
                        binding.chkPrivacy.setBackgroundResource(R.drawable.ic_check_circle_checked)
                    }else{
                        binding.chkPrivacy.setBackgroundResource(R.drawable.ic_check_circle)

                    }
                    if(terms && privacy){
                        presenter?.onAgree()
                    }
                }
~~~

간단합니다. 체크박스가 체크 혹은 체크 해제가 될 때마다 들어오게 되고, 각각 두개의 체크박스가 체크가 되어있으면 presenter의 onAgree를 호출하게 끔 됩니다.

여기서 중요한 부분은 체크박스가 체크 혹은 체크 해제가 될때마다 해당 이벤트를 호출하게끔 된다는 겁니다.
그럼 우리는 전체 동의 버튼을 클릭시, 어떻게 구현해야 할까요?
presenter 인스턴스의 onAgree 메소드를 중복 호출하기 보다는 두 개의 체크박스를 체크하게끔 해주면 해당 이벤트가 발생하게 되고, onAgree 메소드가 호출이 될 것입니다.


~~~java
        binding.btnAllCheck.setOnClickListener {
            binding.chkTerms.isChecked = true
            binding.chkPrivacy.isChecked = true
        }
~~~

위와 같이 말이죠.

이러한 부분들을 응용하면 더 많은 부분들에서 좀 더 편리하게 이용할 수 있을 것 같습니다.

2013년 넷플릭스에서 RxJava를 내놓은 이후에 Rx는 모바일 개발자라면 필수로 해야하는 부분이 되었습니다.
콜백지옥에서 벗어날 수 있다는 점에서만 봐도 Rx는 굉장히 매력적이라고 볼 수 있을 것 같습니다.

감사합니다.

