---
layoout: post
title:  "Gradle Version"
crawlertitle: post_android
date:   2022-04-24 17:05:00 +0900
categories: android
summary: "Android Studio에서 사용하는 필요 Gradle Version에 대한 설명"
---  
- Android Studio에서는 Gradle을 기반으로 빌드하므로 Android Gradle Plugin이 제공됩니다.  
- 간혹 Android Studio 업데이트에 따라 Android Gradle Plugin version과 Gradle version의 Sync error가 발생합니다.
- Gradle Sync error 발생시 Android Developer의 Android Gradle plugin release notes를 참고하여 Android Gradle plugin version과 Gradle version을 맞추면 됩니다.  
- Android Gradle plugin version을 변경하려면, **build.gradle** 내 **classpath**를 수정하면 됩니다.  
  - 예 : {project_root_path}/build.gradle  

~~~gradle  
buildscript {
    ext.kotlin_version = "1.3.72"
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:4.1.2"
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}

allprojects {
    repositories {
        google()
        jcenter()
    }
}
~~~  

- Gradle version을 변경하려면, **gradle-wrapper.properties** 내 **distributionUrl**를 수정하면 됩니다.  
  - 예 : {project_root_path}/gradle/wrapper/gradle-wrapper.properties  

~~~properties  
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-6.5-bin.zip
~~~  

##### 참고  
- [Android Gradle plugin release notes-Update gradles](https://developer.android.com/studio/releases/gradle-plugin#updating-gradle)  
- [Gradle Version](https://services.gradle.org/distributions/)  