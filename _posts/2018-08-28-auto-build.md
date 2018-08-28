---
layout: post
title: 앱 공장 만들기
category: android
tags: [android]
---

## 앱 공장장 되어보기
해당 포스팅은 2016년 8월에 작성된 사내 코드 리뷰 데이때 사용된 자료를 재구성한 내용입니다.

안녕하세요. 모바일팀의 SH입니다.

개발자로써 일을 하다보면, 정말 뜻하지 않은 일을 하기도 합니다. 저는 그 중 하나가 하나의 앱을 공장처럼 찍어내는 일인데요.
정말 뜻하지 않은 일이라도 월급을 주면 해야하는 수밖에 없습니다.. 

만약 하나의 앱을 만들었는데, 여기서 아이콘, 이미지 등만 바꿔서 다른 이름의 앱으로 공장처럼 찍혀내야 한다면 어떨까요

보통 하나의 앱을 빌드하는데 걸리는 시간은 5분이라고 계산을 해보면 10개면 거의 1시간이 걸립니다.

하루에 8시간을 일하는데, 하루종일 빌드한다고 해도 우리는 92개밖에 빌드를 못하네요.

만약에 200개 정도의 앱이 배포가 되었다고 칩시다. 그런데 만약에, 크리티컬한 이슈가 발생이 되어서 새로 업데이트를 해야한다면 16시간 넘게 꼬박 빌드만 해야 가능한 시간입니다.

게다가 하나하나 코드까지 수정한다면 16시간이 아니라 160시간정도 걸릴지도 모릅니다.

그래서 우리는 **빌드 자동화**를 적극적으로 검토해봐야 합니다.

하나의 앱만 관리하는 빌드 자동화에서는 단지 gradle을 활용해서 만들면 됩니다. 
하지만 아까 위에 설명한대로 우리는 하나의 앱에서 패키지, 아이콘, 이름 등만 바꿔가면서 계속해서 찍어내는 앱을 만들어내야 합니다..ㅠㅠ

~~정말 하기싫지만, 월급을 받으니 해야합니다..~~

일단 앱을 뜯어고쳐서 모듈화를 합니다.

모듈화를 한다면, 껍데기 앱에서는 단지 상속만 하면 됩니다.

그런 다음에 빌드만 하면 바로 패키지명만 다른 새로운 앱이 만들어집니다. 개꿀

보통의 앱은 아래와 같은 워크플로우를 가지고 있습니다.
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-wkD2mDUmCs4iF92M" height="100%" viewBox="0 0 230.40625 106" style="max-width:230.40625px;"><g><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath" style="opacity: 1;"><path class="path" d="M85.0625,43L110.0625,43L135.0625,43" marker-end="url(#arrowhead964)" style="fill:none"></path><defs><marker id="arrowhead964" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g></g><g class="nodes"><g class="node" id="A" transform="translate(52.53125,43)" style="opacity: 1;"><rect rx="5" ry="5" x="-32.53125" y="-23" width="65.0625" height="46"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-22.53125,-13)"><foreignObject width="45.0625" height="26"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Splash</div></foreignObject></g></g></g><g class="node" id="B" transform="translate(162.734375,43)" style="opacity: 1;"><rect rx="5" ry="5" x="-27.671875" y="-23" width="55.34375" height="46"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-17.671875,-13)"><foreignObject width="35.34375" height="26"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Main</div></foreignObject></g></g></g></g></g></g></svg></div>
그럼 상속받은 껍데기에서는 Splash에 사용되는 Activity만 상속받는다면, 게임 끝입니다.

껍데기에서는 단지 액티비티 하나만 가지고 있는 초간단 코드가 됩니다.

자 이제 앱의 준비는 끝났습니다.

그런데, gradle을 이용해서 빌드는 할 수 있지만, 우리는 껍데기를 계속해서 만들어내야 합니다.

그럴땐 android 커맨드를 이용해서 프로젝트를 생성하면 됩니다.

하지만, **Android SDK tools 26.0.1**부터는 android 커맨드를 이용하여 프로젝트 생성은 **deprecated** 되었습니다.

Android Stuido를 이용해서 프로젝트를 생성하거나, 기존에 생성된 프로젝트를 스크립트를 통해 복붙하면서 이름을 바꿔줘야 합니다.

하지만 Android 사이트에서 예전 버전의 tools를 제공하므로 따로 저장해서 사용하는걸 추천합니다.

이런형태로 프로젝트를 생성 후 쉘 스크립트를 통해 java파일과 gradle 파일 등을 수정 후 gradle 을 이용하여 빌드를 하면 apk가 나오게 됩니다.

이렇게 나온 apk는 Google에서 제공해주는 api 를 이용하여 스토어로 바로 등록을 할 수도 있습니다.

즉, 전체적인 워크플로우는 아래와 같습니다.

<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-F6e8ESuqvj6h1h11" height="100%" viewBox="0 0 634.1875 106" style="max-width:634.1875px;"><g><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath" style="opacity: 1;"><path class="path" d="M138.828125,43L163.828125,43L188.828125,43" marker-end="url(#arrowhead981)" style="fill:none"></path><defs><marker id="arrowhead981" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M333.625,43L358.625,43L383.625,43" marker-end="url(#arrowhead982)" style="fill:none"></path><defs><marker id="arrowhead982" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M478.5625,43L503.5625,43L528.5625,43" marker-end="url(#arrowhead983)" style="fill:none"></path><defs><marker id="arrowhead983" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g></g><g class="nodes"><g class="node" id="B" transform="translate(79.4140625,43)" style="opacity: 1;"><rect rx="5" ry="5" x="-59.4140625" y="-23" width="118.828125" height="46"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-49.4140625,-13)"><foreignObject width="98.828125" height="26"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">android 커맨드</div></foreignObject></g></g></g><g class="node" id="C" transform="translate(261.2265625,43)" style="opacity: 1;"><rect rx="5" ry="5" x="-72.3984375" y="-23" width="144.796875" height="46"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-62.3984375,-13)"><foreignObject width="124.796875" height="26"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">gradle 및 java 수정</div></foreignObject></g></g></g><g class="node" id="D" transform="translate(431.09375,43)" style="opacity: 1;"><rect rx="5" ry="5" x="-47.46875" y="-23" width="94.9375" height="46"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-37.46875,-13)"><foreignObject width="74.9375" height="26"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">gradle 빌드</div></foreignObject></g></g></g><g class="node" id="E" transform="translate(561.375,43)" style="opacity: 1;"><rect rx="5" ry="5" x="-32.8125" y="-23" width="65.625" height="46"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-22.8125,-13)"><foreignObject width="45.625" height="26"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">후 처리</div></foreignObject></g></g></g></g></g></g></svg></div>

스윗트래커에서는 기본적으로 맥을 사용하고 젠킨스에도 올려야 하므로 편하게 쉘 스크립트를 이용하여 전체적인 스크립트를 만들었습니다.

게다가 젠킨스에도 올리고 모바일 개발자들의 로컬에도 설정을 해야하므로 이런 설정에 관련된 부분들은 개인이 커스텀 할 수 있게 두 개의 쉘스크립트 파일로 나눴습니다.

개인이 설정하는 쉘 스크립트는 setting.sh로 만들어 개인이 직접 셋팅을 하게끔 해줍니다.

```
#모듈화가 존재하는 경로
defaultPath=/Users/ichungseob/Documents/android
#드롭박스 위치
destinationPath=/Users/ichungseob/Dropbox/quickr
#Androoid SDK위치
androidSDKPath=/Users/ichungseob/Documents/android
#프로젝트를 만들 위치
quickrProjectPath=/Users/ichungseob/Documents/quickrProject
```
이렇게 setting.sh 를 만들어 놓으면 다른 쉘 스크립트에서는 간단하게 아래와 같이 선언하면 사용가능합니다.
```
source setting.sh
```

그리고 프로젝트 이름 등은 외부에서 넣어주게끔 해주면 됩니다. 

```
local projectName=$1
```
이런식으로 말이죠. 

그런 후에 아래와 같이 android 커맨드를 사용하여 프로젝트를 생성하면 됩니다.
```
$androidSDKPath/sdk/tools/android create project --activity $mainActivity --package $packageName --target $androidSDK --path $quickrProjectPath/$projectName --gradle --gradle-version $gradleVersion
```
이렇게 하면 $packageName에 선언해 놓은 패키지를 가진 프로젝트가 생성이 됩니다.

이후 **echo**를 이용하여 파일 하나하나 수정해줘야 합니다.

수정해줘야 하는 파일은 꽤 많습니다.

build.gradle, local.properties, settings.gradle, mainActivity.java, strings.xml, AndroidManifest.xml 를 수정해줘야 합니다.

어렵지는 않은데 노가다로 작성을 해줘야 해서 화가 나지만, 월급을 받으니 우리는 해야했습니다.
예를 들어 build.gradle 이라면 아래와 같이 말이죠.
```
...
		echo "\n"apply plugin: \'com.android.application\' >> $buildGradlePath
		echo "\n"android { >> $buildGradlePath
		echo "\t"compileSdkVersion 23 >> $buildGradlePath
		echo "\t"buildToolsVersion \"23.0.2\" >> $buildGradlePath
		echo "\n\t"defaultConfig { >> $buildGradlePath
		echo "\t\t"applicationId \"$packageName\" >> $buildGradlePath
		echo "\t\t"minSdkVersion 14 >> $buildGradlePath
		echo "\t\t"targetSdkVersion 23 >> $buildGradlePath
		echo "\t\t"versionCode $applicationVersionCode >> $buildGradlePath
		echo "\t\t"versionName \"$applicationVersionName\" >> $buildGradlePath
		echo "\t\t"multiDexEnabled true >> $buildGradlePath
...
```

이러한 부분은 미리 샘플 프로젝트를 만들어놓고 보면서 코드를 복붙하면 편합니다.

그리고 아이콘 등 이미지를 셋팅해줍니다.

이 부분도 쉘스크립트가 해줍니다.

```
...
	hdpiPath=$destinationPath/$folderlocate/$projectName/mipmap-hdpi/ic_launcher.png
	mdpiPath=$destinationPath/$folderlocate/$projectName/mipmap-mdpi/ic_launcher.png
	xhdpiPath=$destinationPath/$folderlocate/$projectName/mipmap-xhdpi/ic_launcher.png
	xxhdpiPath=$destinationPath/$folderlocate/$projectName/mipmap-xxhdpi/ic_launcher.png
	xxxhdpiPath=$destinationPath/$folderlocate/$projectName/mipmap-xxxhdpi/ic_launcher.png
	mipmapHdpiPath=$quickrProjectPath/$projectName/src/main/res/mipmap-hdpi
	[ ! -d $mipmapHdpiPath ] && mkdir $mipmapHdpiPath
	mipmapMdpiPath=$quickrProjectPath/$projectName/src/main/res/mipmap-mdpi
	[ ! -d $mipmapMdpiPath ] && mkdir $mipmapMdpiPath
	mipmapXdpiPath=$quickrProjectPath/$projectName/src/main/res/mipmap-xhdpi
	[ ! -d $mipmapXdpiPath ] && mkdir $mipmapXdpiPath
	mipmapXxdpiPath=$quickrProjectPath/$projectName/src/main/res/mipmap-xxhdpi
	[ ! -d $mipmapXxdpiPath ] && mkdir $mipmapXxdpiPath
	mipmapXxxdpiPath=$quickrProjectPath/$projectName/src/main/res/mipmap-xxxhdpi
	[ ! -d $mipmapXxxdpiPath ] && mkdir $mipmapXxxdpiPath

	cp $hdpiPath $mipmapHdpiPath/ic_launcher.png
	cp $mdpiPath $mipmapMdpiPath/ic_launcher.png
	cp $xhdpiPath $mipmapXdpiPath/ic_launcher.png
	cp $xxhdpiPath $mipmapXxdpiPath/ic_launcher.png
	cp $xxxhdpiPath $mipmapXxxdpiPath/ic_launcher.png
    cp $btnQuickCallPath $drawablePath/btn_quick_call.png
	cp $btnHwamulCallPath $drawablePath/btn_hwamul_call.png
	cp $btnMobilePath $drawablePath/btn_mobile.png
	cp $btnHomepagePath $drawablePath/btn_homepage.png
...
```

그리고 마지막으로 아래와 같이 gradle을 사용하여 빌드를 합니다.

```
./gradlew assembleRelease
```

이렇게 하면 프로젝트 내 build/outputs/apk/release/ 에 프로젝트 이름.apk 로 apk 가 생성이 됩니다.

물론 위에 설명한 부분들을 제외한 복잡한 부분들이 쉘 스크립트내에 들어가있지만, 간략하게 워크플로우를 그려보자면 위와 같습니다.

이렇게 만들어진 .sh 를 로컬로 테스트 후에 이제 젠킨스로 올려놓으면 됩니다.

젠킨스의 구성메뉴에서 매개변수를 설정할 수 있습니다.

그렇기 때문에 아까 위에 설명한대로 외부에서 파라미터를 받아서 처리를 해주면 굉장히 편리하게 해줄 수 있죠.

젠킨즈 자체적으로 git도 연동을 해놓을 수 있기 때문에 아주아주 편리하게 모든걸 처리 할 수 있습니다.

해당 스크립트를 이용하여 빌드를 하기전에 git pull 진행 후 빌드하는게 가능합니다.

이런저런 간단한 셋팅들을 끝내놓고 Excecute Shell 에 아래와 같은 커맨드를 입력해놓으면 끝나게 됩니다.

```
sh /var/lib/jenkins/workspace/quick/quickrBuildScript.sh $projectName $isUpdate $isAllUpdate
```

$projectName과 $isUpdate, $isAllUpdate는 외부에서 받는 매개변수로 설정을 해놓으면 됩니다.

즉, 프로젝트 이름과 업데이트인지 아닌지의 여부를 받는 내용입니다.

이렇게 해놓으면 개발자가 아닌 사람들도 어디에서든 apk 를 빌드할 수 있습니다.

이런 개발이 많아지면 안되겠지만, 어쩔수 없이 이런것들을 개발해야 하는 상황이 온다면 빌드자동화를 통해서 우리가 할일을 대폭 줄여야 합니다.

개발자가 개발을 해야지 앱만 빌드 할 수는 없으니깐요.

스윗트래커에서도 해당 스크립트를 만든 후 뒤에 이런 저런 기능들이 붙여져서 700라인이 넘는 거대한 스크립트가 되었습니다만, 여전히 잘 동작하고 개발자가 할 일을 대폭 줄여줬습니다.

아마 저의 다음 포스팅은 스마트택배에 **RxJava**와 **Retrofit** 적용하여 **API 호출 뎁스 줄이기**가 될 것 같습니다.

감사합니다.


