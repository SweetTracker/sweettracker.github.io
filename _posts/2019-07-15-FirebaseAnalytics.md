---
layout: post
title: Firebase Analytics, BigQuery없이 어디까지 가능할까
category: Android 
tags: [mobile, android, analytics, firebase, google, GA, FA]
---



## Firebase Analytics 사용법



 안녕하세요. 스윗트래커 모바일팀 MJ입니다.

 첫 포스팅으로 **애널리틱스**에 대해 다뤄보고자 합니다. 애널리틱스는 사용자의 데이터를 수집해서 수행결과를 측정하고 추후 개선사항이나 마케팅 타겟을 설정하여 더욱 효과적으로 비즈니스 성과를 낼 수 있도록 도와주는 분석 도구입니다. 



 현재 스마트택배는 **Google Analytics (GA)**를 통해 사용자의 데이터를 수집하여 분석하였으나, 2019년 10월부터 지원이 중단되어 **Firebase Analytics (FA)** 에 대해 알아보았습니다.

- 이전 데이터는 2020년 1월 31일까지 액세스 가능합니다.

- GA가 중단됨에 따라 여러 애널리틱스 도구들 중 FA로 많은 분들이 전환할 것이라 예상됩니다.







>  애널리틱스 비교

|                              GA                              |                              FA                              |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <br />웹 중심 + 모바일 <br />**스크린 뷰** 중심<br />화면 단위로 사용자의 유입 경로 확인 가능<br />샘플 데이터 수집 후 통계<br /> | <br />모바일만 가능<br />**이벤트** 중심<br />설계된 이벤트로 사용자의 유입경로를 정의 (Funnel 기능)<br />모든 데이터 수집 = 상대적으로 느림<br /> |



 **스크린 뷰** 중심으로 이루어진 GA에서 앱에서 발생하는 **이벤트** 중심인 FA를 사용하게 되면서 큰 차이를 보이게 되는데, FA로 이벤트가 어떻게 전송될까요?





 바로,  **bundle**을 생성하여 아이디, 수량, 아이템 등 원하는 **매개변수**를 담고 원하는 **이벤트 명**으로 전송합니다.

 필요 부분에 아래와 같은 코드를 작성하여 FA에 전송하며, 프로젝트당 500개까지 이벤트를 보고할 수 있고 최대 25개의 매개변수를 담을 수 있습니다.



~~~kotlin
val bundle = Bundle()
bundle.putString("아이디", id)
bundle.putLong("수량", quantity)
bundle.putString("아이템", item)
firebaseAnalytics.logEvent("이벤트명", bundle)
~~~



 전송되는 원리를 알았으니, 다음으로 FA에서 말하는 이벤트의 3가지 종류를 알아보겠습니다.









## 1. 자동 추적 이벤트

Firebase SDK를 사용하기만 해도 별도의 코드 없이 자동으로 수집되는 이벤트입니다.



> ####screen_view
>
> 화면 전환이 있을 경우
>
> IOS는 UIViewController, 안드로이드는 Activity만 측정되는데, 보기 쉽도록 화면 이름을 직접 설정하려면 별도 코드 필요



> #### first_open 
>
> 앱 설치 또는 재설치 후 처음으로 앱을 실행할 경우



> #### session_start
>
> 세션 제한시간(30분)보다 길게 비활성 상태였다가 최소 세션 기간(10초)보다 오래 사용할 경우. 즉, 실 사용 확인 가능



> #### app_update
>
> 앱 업데이트 후 실행 될 경우 (앱 버전이 매개변수로 수집)



[모든 자동 추적이벤트 보러가기](https://support.google.com/firebase/answer/6317485?hl=ko)









## 2. 기본 제공 이벤트

파이어베이스에서 정의해 놓은 이벤트로 필요에 따라 코드를 작성하여 사용할 수 있습니다.



~~~kotlin
val bundle = Bundle()
bundle.putString(FirebaseAnalytics.Param.ITEM_ID, id)
bundle.putString(FirebaseAnalytics.Param.ITEM_NAME, name)
bundle.putString(FirebaseAnalytics.Param.CONTENT_TYPE, "image")
firebaseAnalytics.logEvent(FirebaseAnalytics.Event.SELECT_CONTENT, bundle)
~~~



 위 코드로 보면 Item_id / item_name / content_type이 매개변수가 될 것이고, select_content가 이벤트 명입니다.

공식 문서를 확인해보면,





![basic_event](/assets/images/basic_event.png)



 예를 들어, **ADD_TO_WISHLIST**의 경우 item_id / item_name / item_cotegory / quantity 라는 매개변수가 반드시 작성되어야 하며 매개변수 오른쪽에 **(optional)** 이라고 적힌 매개변수는 필요에 따라 사용하시면 됩니다. (그 외에 또 추가하고 싶은 매개변수가 있어도 사용하실 수 있습니다.)

 각 매개변수 옆에 어떤 형인지 작성되어있으므로 전송할 때 데이터형에 맞게 put 할 수 있도록 합니다.



[모든 제공 이벤트 보러가기](https://firebase.google.com/docs/reference/android/com/google/firebase/analytics/FirebaseAnalytics.Event)

[모든 제공 매개변수 보러가기](https://firebase.google.com/docs/reference/android/com/google/firebase/analytics/FirebaseAnalytics.Param)









## 3. 커스텀 이벤트

제공된 이벤트 외에 원하는 모양으로 이벤트를 만들 수 있습니다.

위 쇼핑백을 예로 들은 코드가 바로 커스텀 이벤트이며, 원하는 **이벤트 명**으로 전송하는 이벤트를 말합니다.





> **Value_테스트** 이벤트의 보고서

![fa_value](/assets/images/fa_value.png)





 다른 예로, **Value_테스트** 라는 이벤트에 **FirebaseAnalytics.Param.VALUE** 라는 제공 매개변수를 담아 전송해보았습니다. 

 모든 이벤트 보고서에 디폴트로 적혀있는 **값**이라는 매개변수를 사용하기 위해  **FirebaseAnalytics.Param.VALUE** 매개변수를 사용했습니다.

  — 여기서 **값**이란 ?    long 또는 double 형으로 전달 할 수 있는 이벤트의 누적 값. 즉, **전송된 값의 합**을 말합니다.









### 이벤트의 종류는 알겠는데, 왜 나누나요?



 자동 추적 이벤트는 알아서 측정이 된다지만, 기본 제공 이벤트와 커스텀 이벤트는 만들어지는 보고서의 형태가 다르기 때문입니다. 

 최대 500개까지 전송할 수 있는 이벤트를 최대한으로 활용하기위해 알아둬야 되는 사실 중 하나 - 이벤트명만 보낸, 즉 가장 기본 요소만 보냈을 때 측정되는 것은, 



- 이벤트 수
- 사용자 수
- 사용자 당 이벤트 수
- 값
- 지난 30분 동안의 이벤트
- 이벤트 위치
- 이벤트 인구 통계
- 세션당 이벤트 수



 총 8가지 입니다. 그럼 bundle에 같이 전송한 매개변수는 어떻게 확인할 수 있을까요? 

 이벤트 별로 **매개변수 보고서를 설정**해야 합니다. 프로젝트 당 **최대 10개의 고유 텍스트 매개변수**, **40개의 고유 숫자 매개변수** 총 50개를 가질 수 있습니다. 즉, 모든 이벤트를 합쳐 String으로 보낸 매개변수는 10개까지, 나머지 숫자형은 40개까지 보고서에서  확인 가능합니다. (생각보다 큰 제한………..)

 이벤트 당 최대 25개의 매개변수를 보낼 수 있지만 이러한 제한이 있기 때문에 모든 매개변수를 확인하는 것이 어렵습니다.

 다행히 기본 제공 이벤트의 경우 몇가지 매개변수는 매개변수 카운트에 포함되지 않고 보고서에 보여주긴 합니다. (하지만 이벤트별로 디폴트로 보여주는 매개변수는 다 다르기때문에 무엇을 보여주는지 직접 확인해보는 수 밖에……….)





> 매개변수 보고서 설정 방법

![fa_setting](/assets/images/fa_setting.png)



![fa_setting2](/assets/images/fa_setting2.png) 



 select_content 기본 제공 이벤트를 확인해보면, content_type 과 item_id 라는 매개변수는 디폴트로 들어가 있어서 보고서에서 삭제도 불가하며, 매개변수로 카운트가 되지 않습니다. Item_name 의 경우 보고서에 추가해 놓은 것으로 텍스트 매개변수로 카운트 되며 삭제 가능합니다. 

 **커스텀 이벤트**의 경우에는 디폴트로 설정되는 것은 없으며 모든 매개변수는 보고서 설정이 필요합니다.

 하단에 등록한 전체 매개변수의 개수를 확인할 수 있으며, 개수는 한 이벤트 당이 아닌 모든 이벤트를 합친 카운트입니다.



#### _물론! 모든 매개변수를 확인할 수 있는 방법이 있습니다._

 Firebase를 Blaze 요금제로 업그레이드한 후 **BigQuery **라는 유료 서비스를 사용하여 쿼리문을 통한 모든 데이터 확인이 가능합니다. 이를 보고서와 같이 시각화하기 위해 구글에서 제공하는 **데이터 스튜디오**를 사용하면 되구요.









### 매개변수를 보고서에 설정해놓으면 바로 데이터를 확인할 수 있나요?



 아닙니다. 보고서 설정 후에 값이 제대로 측정되며, 보고서 설정하기 이전에의 값은 때에 따라 (not set)으로 보입니다. 즉, 보고서 설정을 하지않으면 매개변수를 넣는다 한들 제대로 측정되지 않습니다.

 따라서 데이터를 보고싶을 때 매개변수를 보고서에 설정하면 최대 50개를 설정하는 제한이 상관없지 않을까? 할 수 있겠지만 불가능합니다. 

 매개변수를 보고서에서 삭제하면 등록된 모든 데이터가 삭제된다는 안내가 나오지만, 바로 다시 등록하면 그대로 살아있습니다. 하지만 삭제 후 24시간이내로 데이터는 사라집니다.  

 추가적으로, 이벤트가 보고서에 보여지기까지도 최대 24시간이 걸리며 추가로 매개변수를 보고서에 설정하면 또 최대 24시간이 걸립니다. 이 사이의 값이 (not set)으로 보여지고 등록 된 후에 값은 제대로 측정됩니다. (확인된 것으로는 이벤트를 전송했을 때 다음 날 아침에 확인할 수 있었습니다.)









 ### 그러면 다음날이 될 때까지 이벤트가 제대로 들어갔는지 확인할 수 없나요?



 **DebugView** 를 사용하면 가능합니다.

> [Firebase DebugView 자세히 보기](https://firebase.google.com/docs/analytics/debugview?hl=ko)

![debug_view](/assets/images/debug_view.png)



안드로이드 DebugView 실행 코드

~~~
./adb shell setprop debug.firebase.analytics.app <package_name> //실행
./adb shell setprop debug.firebase.analytics.app .none. //중지
~~~



구글링해보니 꽤 여기서 잘 안되는 개발자가 있어서 덧붙이자면, 먼저 터미널에서 **adb**가 있는 폴더를 찾아야 합니다. 

 — **adb** 란?   안드로이드 디버그 브릿지로 에뮬레이터나 연결된 안드로이드 기기와 통신할 수 있는 명령줄 도구입니다.



 보통 android_sdk/**platform-tools** 에 adb가 있으며 이는 각각 위치가 다르니 폴더를 찾아서 ls 명령으로 adb가 존재하는지 확인한 후 DebugView를 실행합니다. 

 실행한 후 아무 변화가 없어보이지만, 이제 **firebase analytics > DebugView**에서 몇 초 후 확인하면 안드로이드 기기가 인식되고 이벤트 호출을 확인 할 수 있습니다. 

 매개변수의 값이 잘 안들어왔다면 새로고침(꼭 해보세요 !!!)을 해보고, 그래도 잘 안들어왔다면 코드상 문제가 있는 것입니다.









### 화면 이름을 보기 쉽게 설정하려면 ?



 **screen_view** 라는 자동 추적 이벤트는 안드로이드에서 Activity의 class명으로 추적됩니다. 따라서 해당 프로젝트의 개발자가 아닌 이상 어떤 화면인지 알아보기 어려우므로 수동으로 화면 이름을 설정해줄 필요가 있습니다.



~~~kotlin
firebaseAnalytics.setCurrentScreen(activity, "화면 이름", null)
~~~



설정 후 screen_view 보고서의 사용자 참여도 > 화면 이름을 선택하여 확인할 수 있습니다.









 ### 사용자의 유입 경로를 확인하려면 ?



이벤트를 이어서 유입 경로를 확인할 수 있는 **Funnels**를 사용하면 됩니다.

![funnel](/assets/images/funnel.png)



![funnel2](/assets/images/funnel2.png)



GA과 비교하자면 **화면** 단위가 아니라 **이벤트** 단위입니다. 어떠한 이벤트에서 사용자가 이탈되는지 확인 가능합니다. 이는 프로젝트당 최대 200개까지 만들 수 있습니다. 









---



 여기까지 GA와의 비교를 토대로 FA를 사용하는 방법에 대해 알아보았습니다. SDK에 탑재만 해도 자동으로 추적해주는 이벤트와 적절히 사용하면 아주 큰 효과를 볼 수 있는 기본 제공 이벤트, 그 외 커스텀 이벤트를 사용하여 필요 부분을 측정하여 유연하게 FA를 사용할 수 있도록 많은 개발자에게 도움이 되었으면 합니다 :) 



감사합니다.



