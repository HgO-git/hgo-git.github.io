---
layoout: post
title:  "AppCompatActivity"
crawlertitle: post_android
date:   2022-05-21 15:48:00 +0900
categories: android
summary: "Android의 AppCompatActivity에 대한 설명"
---  
Android 3.0(API 11)부터 기본 테마를 사용하는 Activity에 ActionBar가 제공됩니다.  
Android가 출시를 거듭할수록 다양한 기능들이 ActionBar에 추가되다보니 Native ActionBar는 단말기에서 사용하는 Android OS에 따라 다르게 작동하게 되었습니다.  
각 Android OS마다 ActionBar 기능과 커버하는 API Level이 다르므로 다양한 Android OS에서 동일한 동작이 가능하려면 하위 버전 Android OS까지 커버가 가능해야 하는데 이때 사용 가능한 Activity가 AppCompatActivity입니다.  
게임처럼 ActionBar가 필요없는 경우 상관없습니다. 하지만 ActionBar가 필요한 일반 앱인 경우 최신 Android OS가 설치된 단말기에서만 100% 구동된다는 확신이 없으므로 사용하는 것이 좋습니다.  

아래는 2022년 05월 기준으로 OS별 점유율입니다.  
![Android OS별 점유율](/assets/images/post_android/post_android_appcompatactivity_os_distribution.png)

<br>

예전에는 Android Developer 사이트에서 확인이 가능했으나 현재는 Android Studio에서 New Project시 확인 가능합니다.  

<br>

##### 참고  
- [AppCompatActivity](https://developer.android.com/reference/androidx/appcompat/app/AppCompatActivity)  
- [Android Distribute Dashboards](https://developer.android.com/about/dashboards)