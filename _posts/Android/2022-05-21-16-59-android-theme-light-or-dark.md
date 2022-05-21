---
layoout: post
title:  "Theme가 Light인지 Dark인지 확인"
crawlertitle: post_android
date:   2022-05-21 16:59:00 +0900
categories: android
summary: "Theme가 Light Mode인지 Dark Mode인지 확인하는 방법"
---  
눈 건강, 배터리 절약과 같은 이점으로 Dark Mode가 사용되면서 앱에서도 Dark Mode에 대한 처리가 필요합니다.  
대부분 자동으로 처리되지만 커스텀하게 사용한 부분이 있다면 현재 Theme가 Light Mode인지 Dark Mode인지 확인이 필요합니다.  

- java에서 확인하는 방법  

~~~java
int currentNightMode = getContext().getResources().getConfiguration().uiMode & Configuration.UI_MODE_NIGHT_MASK;

switch (currentNightMode) 
{
    case Configuration.UI_MODE_NIGHT_NO:
        // Night mode가 활성화되어 있지 않습니다.
        break;
    case Configuration.UI_MODE_NIGHT_YES:
        // Night mode가 활성화되어 있습니다.
        break;
}    
~~~  

- kotlin에서 확인하는 방법  

~~~kotlin
var flags : Int = decorView.systemUiVisibility  

when (context.resources.configuration.uiMode and Configuration.UI_MODE_NIGHT_MASK)
{
    Configuration.UI_MODE_NIGHT_NO ->
    {
        // Night mode가 활성화되어 있지 않습니다.
    }
    Configuration.UI_MODE_NIGHT_YES ->
    {
        // Night mode가 활성화되어 있습니다.
    }
}
~~~  

##### 참고  
- [Android Configuratoin](https://developer.android.com/ndk/reference/group/configuration)  
- [Android UiModeManager](https://developer.android.com/reference/android/app/UiModeManager)  