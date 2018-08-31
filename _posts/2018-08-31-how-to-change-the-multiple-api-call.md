---
layout: post
title: Retrofit과 RxJava로 연쇄 API 호출 코드 수정하기
category: android
tags: [RxJava, Retrofit]
---




## Retrofit과 RxJava로 연쇄 API 호출 코드 수정하기



안녕하세요. 모바일 팀의 sh입니다.

스마트택배는 서버와의 api 통신을 많이 합니다.

각각 택배사들에 대한 정보들도 받아와야 하고, 쇼핑몰 관련 정보들, 상태값 등등 정말 많은 데이터의 동기화를 위해 api 통신을 합니다.

당연히 버전관리를 통해서 통신을 최소화하여 네트워크 사용량을 줄였습니만 이렇게 api 호출이 많다보니 자연스럽게 연쇄 api 호출의 이슈도 발생하였습니다.

왜냐면, 중간에 하나의 api라도 비정상적으로 작동하게끔 한다면 서비스를 제공하면 안되기 때문이죠.

쇼핑몰 불러오가 대표적인 경우인데요. 이 서비스는 아래와 같이 총 5개의 api를 연쇄적으로 호출하고 있습니다.

1. 쇼핑몰 상태값
2. 쇼핑몰 정보값
3. 라이브러리 정보값
4. 라이브러리 다운로드
5. sns로그인 관련값

현재 Handler를 이용하여 데이터를 받게끔 되어있는데, 1번 api 호출 후 Handler로 전달된 메세지 확인 후 2번 api 호출. 그리고 Handler로 전달된 Message 확인 후 3번 api호출..

이렇게 해서 총 5개의 api를 호출하고 있습니다.

그럼 코드는 어떻게 될까요.

~~~java
getShoppingMallStatus(new Handler.Callback(){
    @Override  
    public boolean handleMessage(Message msg){
        ...data처리
        getShoppingMallInfomation(new Handler.Callback()){
            @Override 
            public boolean handleMessage(Message msg){
                ...data처리
                ...api호출
            } 
        }
    }
});
~~~

이렇게 뎁스가 깊게 코드를 작성하게끔 하다보면 유지보수에서도 이슈가 발생하게끔 됩니다.

그리고 눈으로 보기에도 가독성이 현저히 떨어지게끔 됩니다.

결국 getShoppingAllStatus 메소드를 따라 들어가서 확인 후 다시 돌아와서 후처리에 대해서 확인을 해야합니다. 이런식으로 5번을 왔다갔다하면서 코드를 확인해야 하죠.

차라리 interface 를 만들어서 처리를 해줬다면 지금보다는 깔끔하게 보였겠지만, 결국엔 비슷한 문제가 생겼을 것 입니다.

이런식으로 계속해서 기능이 더해진다면 더할나위 없는 끔찍한 혼종의 코드로 변신할겁니다.

![enter image description here](https://lh3.googleusercontent.com/jVm_AMONj0IfSWHJebsdZq6SgLYoKvmg_9x9f9kWCeyBdwNKVRv98UzS4tSzX871ofjg-rw3oAzR)

그러기전에 우리는 얼른 칼을 대야할 것 같습니다.

그런데 가만히 또 api 후처리 코드를 보니, 그 전 api 호출 후 들어온 데이터를 이용해서 다음 api 를 호출하는 개념이 아닙니다.

그냥 단순하게 연달아서 호출만 하고 데이터처리를 하는 식입니다.

그럼 단순하게 카운트를 해주던가, 큐로 핸들러나 인터페이스를 넣으면서 끝날때마다 하나씩 빼주면서 관리해주면 쉽게 해결이 될 것 같습니다.

근데 이런건 개인적으로 좋아하는 방식도 아니고 요새 유행한다는 Retrofit과 RxJava를 이용해서 이걸 해결해볼겁니다.

RxJava와 코틀린과 결합하면 레알 개꿀인데, 부득이한 사정으로 스마트택배에서는 쌩자바로 만들겠습니다.

아무튼간에 칼을 대서 하나씩 변경하기로 했습니다.

일단 interface를 만들어 API를 정의하였습니다.

~~~java
public interface API{  
    @GET("shopping_shops_v3")  
    Observable<ShopsModel> getShopList(@Query("version") String version, @Query("app") String app, @Query("os_version") String os_version);  
  
    @GET("open_status_s")  
    Observable<OpenStatusModel> getShopStatus(@Query("version") String version, @Query("app") String app, @Query("os_version") String os_version);  
}
~~~

Observable 인스턴스로 반환되기 때문에, 이 친구들은 각각 ShopsModel, OpenStatusModel로 발행됩니다.

api 준비는 끝났으니 Retrofit 인스턴스와 API interface를 구현해야합니다.

Retrofit 인스턴스는 Builder 패턴으로 생성하면 됩니다.
~~~java
new Retrofit.Builder().baseUrl(BASEURL).addConverterFactory(GsonConverterFactory.create()).addCallAdapterFactory(RxJavaCallAdapterFactory.create()).build();
~~~
API interface는 Retrofit 인스턴스의 create 메소드를 이용하면 뚝딱 구현부가 생성된 인스턴스로 반환됩니다.
~~~java
mRetrofit.create(API.class);
~~~

엇 가만히 들여다보니 Retrofit 인스턴스와 구현된 API는 할때마다 생성해줄 필요가 없을 것 같습니다
그래서 싱글톤으로 뚝딱 만들어줍니다 ㅎㅎ

구현된 전체코드는 아래와 같습니다
~~~java
public class ShoppingDiaryApi {  
  
    private volatile static ShoppingDiaryApi mInstance;  
    private Retrofit mRetrofit;  
    private API mAPI;  
  
  
    private ShoppingDiaryApi(){  
        mRetrofit = new Retrofit.Builder().baseUrl(BASEURL).addConverterFactory(GsonConverterFactory.create()).addCallAdapterFactory(RxJavaCallAdapterFactory.create()).build();  
        mAPI = mRetrofit.create(API.class);  
    }  
  
    public synchronized static ShoppingDiaryApi getInstance() {  
        synchronized (ShoppingDiaryApi.class) {  
            if (mInstance == null) {  
                mInstance = new ShoppingDiaryApi();  
            }  
            return mInstance;  
        }  
    }
  
    public API getAPI(){  
        return mAPI;  
    }  

    public interface API{  
        @GET("shopping_shops_v3")  
        Observable<ShopsModel> getShopList(@Query("version") String version, @Query("app") String app, @Query("os_version") String os_version);  
  
        @GET("open_status_s")  
        Observable<OpenStatusModel> getShopStatus(@Query("version") String version, @Query("app") String app, @Query("os_version") String os_version);  
    }  
}
~~~

그럼 어디에서든 ShoppingDiaryApi.getInstance()를 통해서 반환받은 ShoppingDiaryApi를 이용해서 사용이 가능할 겁니다.

그럼 이제 정확하게 데이터가 들어오는지 하나씩 만들어봐야 겠습니다.

~~~java
private Observable<OpenStatusModel> getOpenStatusObservable(){  
    ShoppingDiaryApi.API api = ShoppingDiaryApi.getInstance().getAPI();  
    Observable<OpenStatusModel> observable = api.getShopStatus("1", "1", "1");  
    return observable;  
}  

private Observable<ShopsModel> getShopsObservable(){  
    ShoppingDiaryApi.API api = ShoppingDiaryApi.getInstance().getAPI();  
    Observable<ShopsModel> observable = api.getShopList("1", "1", "1");  
    return observable; 
}
~~~
간단하게 Observable을 반환해주는 두개의 메소드를 만들었습니다.

이제 Observable를 구독해야합니다.

우리는 zip이라는 메소드를 사용할 것 입니다.

zip의 경우 Observable의 결합으로, 파라미터로 삽입된 Observable들을 한번에 구독할 수 있습니다.

즉, 우리가 여러개의 api 를 묶어서 호출할때 하던 쓸데없는 카운트 세기라던지, 큐 형태로 핸들러나 인터페이스를 add한 후 하나씩 빼면서 처리하던 역할들을 모두 제거할 수 있습니다.

Observable들을  zip으로 묶으면서 각각의 subscribe를 해주고 싶었으나, 아직은 그 방법을 딱히 모르겠네요. 해당 방법에 대해서 아시는 분이 있으면 댓글로 적어주시면 감사하겠습니다.

이게 가능하다면 zip으로 묶인 Observable의 스케쥴러 관리를 각각 해주게 되므로 좀 더 용이하게 사용이 가능 할 것으로 보입니다.

~~~java
Observable.zip(getOpenStatusObservable(), getShopsObservable(),  
 new Func2<OpenStatusModel, ShopsModel, Boolean>() {  
            @Override  
  public Boolean call(OpenStatusModel openStatusModel, ShopsModel shopsModel) {  
                if(openStatusModel != null){  
                    .. 데이터 저장
                    if(이슈발생시){
                        return false;
                    }
                }  
                if(shopsModel != null){  
                    .. 데이터 저장 
                    if(이슈발생시){
                        return false;
                    } 
                }  
                return true;  
                }})  
        .subscribeOn(Schedulers.newThread())  
        .observeOn(Schedulers.newThread())  
        .subscribe(  
                new Action1<Boolean>() {
                    @Override
                    public void call(Boolean aBoolean) {
                      ..후처리
                    }  
                },
                new Action1<Throwable>() {
                    @Override
                    public void call(Throwable throwable) {  
                        ..에러처리
                    }  
                }, 
                new Action0() {  
                    @Override  
                    public void call() {  
                       ..완료처리
                   }  
       });  
 
~~~
끝!!! 

Observable을 zip으로 묶어 여러개의 Observable을 한번에 처리하게끔 해줍니다.

기존의 Observable과 마찬가지로 처리와 구독을 하게끔 해주는데, zip으로 묶었기 때문에 이것을 한번에 처리해주게끔 해주는 기가막힌 녀석입니다.

주의 할점은 데이터 저장시 에러발생시에 false로 반환했는데, 이러한 경우에도 완료처리까지 동일하게 들어온다는 것입니다. 개발자마다 차이가 있겠지만 차라리 throws Exception으로 처리해서 에러처리에서 받는 것도 좋은 방법입니다.

Observable에 subscribeOn메소드와 observeOn 메소드를 이용해서 각각의 처리와 콜백을 어떤 쓰레드에서 할지를 정할 수 있는데요.

해당 값은 아래 Rxjava와 RxAndroid가 제공해주는 Scheduler로 설정이 가능합니다.

RxJava가 제공해주는 Scheduler는 아래와 같습니다.

-   `Schedulers.computation()`  - 이벤트 룹에서 간단한 연산이나 콜백 처리를 위해서 쓰는 것입니다. I/O 처리를 여기에서 해서는 안됩니다.
-   `Schedulers.from(executor)`  - 특정  `executor`를 스케쥴러로 사용합니다.
-  `Schedulers.immediate()`  - 현재 스레드에서 즉시 수행합니다.
-   `Schedulers.io()`  - 동기 I/O를 별도로 처리시켜 비동기 효율을 얻기 위한 스케줄러입니다. 자체적인 스레드 풀에 의존합니다.
-   `Schedulers.newThread()`  - 항상 새로운 스레드를 만드는 스케쥴러입니다.
-   `Schedulers.trampoline()`  - 큐에 있는 일이 끝나면 이어서 현재 스레드에서 수행하는 스케쥴러.

그리고 RxAndroid에서 제공해주는 Scheduler는 아래와 같습니다.

-   `AndroidSchedulers.mainThread()`  - 안드로이드의 UI 스레드에서 동작합니다.
-   `HandlerScheduler.from(handler)`  - 특정 핸들러  `handler`에 의존하여 동작합니다.

subscribeOn과 observeOn 에서는 말보다는 아래 그림을 보면서 생각하시는걸 추천합니다.

subscribeOn의 경우에는 보통 시작하는 스케쥴러를 말합니다. 즉, 처리를 위한 스케쥴러라고 보셔도 되는데요. 뒤에 observeOn 에서 어떤 스케쥴러로를 선택하기전까지는 해당 스케쥴러로 처리가 되게끔 됩니다.

![enter image description here](https://lh3.googleusercontent.com/cfk5H0eu9fgjdG8fkzWHTyzwcAYblL3t3-VxyHepQ8tSdQj-D4ClHHZrJBd6eqDC8NnrmPF_uRF5)

subscribeOn 호출 시점과 다르게 제일 처음에 파란색으로 표시된 것을 보실 수 있습니다.
그리고 observeOn은 호출 이후에 색이 달라지는 것을 확인 할 수 있습니다.
이러한 부분은 Observable내에서 처리에 대한 스케쥴러를 자유자재로 전환하는게 가능한데요.
예를들면 api -> 매핑 -> ui처리 -> 처리된 값을 이용한 api 등 백그라운드 쓰레드와 UI 쓰레드의 전환을 자유자재로 할 시에 굉장히 유용하게 사용이 됩니다.

이런걸 모르던 시절에는 Handler를 이용하거나, AsyncTask, runOnUiThread 등을 이용해서 처리를 했지만 Rx를 이용하면 편리하게 이용이 가능합니다.

간단하게 예를 들어서 2개의 API를 멀티플하게 호출하는 방법을 보여드렸습니다.
 zip 메소드의 경우 9개의 Observable을 받을 수 있는 메소드까지 오버로딩 되어있으므로, 왠만해선 불편함 없이 사용이 가능할 것으로 보입니다.

스마트택배에 RxJava를 적용하다보니 굉장히 많은 부분들이 유용하게 사용될 것으로 보여집니다.
배송목록에서도 사용되는 쓰레드들도 처리가 쉬울 것으로 보여지구요.
심지어 쇼핑몰 불러오기의 경우에도 굉장히 유용하게 사용이 될 것으로 보여집니다.
유행이니 뭐니를 떠나서 실제로 적용을 해보면 굉장히 편리하다고 느껴집니다.
게다가 코틀린과의 궁합에 있어서도 상상초월이라고 하니, 꽤 기대가 되기도 합니다.