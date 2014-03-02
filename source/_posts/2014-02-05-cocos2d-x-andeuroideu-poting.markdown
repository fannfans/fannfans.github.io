---
layout: post
title: "cocos2d-x 안드로이드 환경 셋팅"
date: 2014-02-05 03:59:28 +0900
comments: true
author: hyonnnnn
categories: [cocos2d-x, init, android]
---
들어가기 전에
-------
Cocos2d-x의 여러 버전을 설치해서 사용해 보았지만 3.0은 아직 안정적이지 않고 개인적으로 2.x버전이 편하기도 해서 2.2.1 버전으로 포스팅을 올린다. 3.0버전도 크게 다르지는 않다.

mac os X에서 안드로이드 프로젝트 환경을 세팅하는 법과 그 과정에서 나를 화나게 만든 에러들을 해결하는 방법에 대해 적어보려고 한다.

안드로이드에서 실행시키기
--------
이클립스(혹은 다른 툴), sdk와 안드로이드 ndk등등은 셋팅되어있다는 가정하에 설명하겠음. 에러에 관해 쓰는게 주 목적이기 때문에 간단히 설명하도록 하겠다.

프로젝트의 proj.android 폴더를 들어가서 **build_native.sh** 파일을 실행 시킨다.

![proj.android](/source/images/android_ex.png)
위와 같은 화면이 보인다면 아래와 같이 입력하면 된다. (짱 친절함)
```
./build_native.sh
```
아래 화면과 같이 나온다면 성공한 것이다.==(Install : 어쩌구 저쩌구가 나오는 것이 핵심 !)==
![install 완료](/source/images/android_install_complete.png)

하지만 아마 이걸 보고있는 당신은 위와 같은 화면이 나오지 않았겠지...

나를 화나게 한 에러들
------------
### 1. error: undefined reference to 어쩌구저쩌구
![android.mk error](/source/images/androidmk_error.png)
위 캡쳐와 비슷하게 생긴 에러가 났다면 아마 당신은 프로젝트에 새로운 cpp 파일을 만들었을 것이다. 에러문을 읽어봐도 알 수 있듯이 레퍼런스가 정의되어있지 않은 것이다. proj.android 폴더안에있는 jni 폴더에 들어가보자. Android.mk 라는 파일이 있을 것이다. 그 파일을 수정하면 된다. 모르겠으면 아래와 같이 입력!
```
vi Android.mk
```
![android.mk error](/source/images/edit_androidmk.png)
상자안에 있는 부분 처럼 새로 추가한 파일 명을 적어주면 된다.
저장 후 다시 빌드 하면 아마 될 것이다.

그리고 혹시 cpp파일을 classes 폴더 안에 새로운 폴더를 만들어서 넣었다면 또 한번 수정해야 한다. 위에 있는 빨간 상자 바로 아래부분에
```
LOCAL_C_INCLUDES := $(LOCAL_PATH)/../../Classes
				 += $(LOCAL_PATH)/../../Classes/폴더명
```
이런 식으로 추가해주면 된다.

### 2. java파일 에러
빌드를 성공하고 떨리는 마음으로 프로젝트를 임포트했는데 빨간 줄들이 뙇!!!

![java error](/source/images/java_error.png)

이 빨간줄들은 cocos2d-x 라이브러리를 참조하지 못하고 있어서 나는 에러들이다.
cocos2d-x 라이브러리를 임포트 해주면 된다!! import 경로는 다음과 같다.
```
cocos2d-x-2.2.1/cocos2dx/platform/android/java
```

### 3. resource 에러

특정 리소스들이 로드되지 않는 경우가 있다. 아마 그 리소스들은 폴더 혹은 그룹안에 들어있을 것이다.

ios에서는 폴더,그룹 둘다 경로 설정을 하지 않아도 알아서 리소스를 찾아온다. 하지만 안드로이드에서는 폴더안에 있을 경우 경로 설정을 해줘야만 한다. 경로 설정이 제대로 된다면 그러한 에러는 사라질 것이다.

마무리
---------
cocos2d-x를 설치하고 프로젝트 세팅하는데 꽤 많은 시간을 들였다. 글로 정리하다 보니 다시 화가 나려고 한다.(ㅎㅎㅎ) cocos2d-x를 처음 접하는 사람들이 세팅하다가 포기하는 일이 없도록... 소중한 시간 허비하지 않도록 조금이나마 도움이 되었으면 좋겠다.
