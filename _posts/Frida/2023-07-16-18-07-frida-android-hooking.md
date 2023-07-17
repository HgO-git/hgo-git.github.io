---
layoout: post
title:  "Android Hooking"
crawlertitle: post_frida
date:   2023-07-16 18:07:58 +0900
categories: frida
summary: "Frida를 통한 Android Hooking 기본"
---  
Frida를 이용하여 안드로이드 앱을 Hooking하는 기본 방법입니다.  
일반적으로 루팅된 단말기로 진행하지만 Frida Gadget을 이용하면 루팅되지 않은 단말기로도 가능합니다.  
PC에서 사용하는 x86/x86_64 에뮬레이터(Bluestacks, Nox, LPPlayer 등)는 **libhoudini.so**을 통해 arm/arm64 앱을 실행합니다. 에뮬레이터의 ABI와 맞는 Frida Server를 실행하는 경우 arm/arm64인 앱의 모듈을 로드할 수 없었으나 Frida 14.2부터 **--realm=emulated** 인자를 설정하여 x86/x86_64 에뮬레이터에서 arm/arm64 앱의 libil2cpp.so 모듈에 접근할 수 있게 되었습니다. 하지만 정상적인 인젝션, 후킹과 같은 처리는 불가능합니다.

##### 1. 루팅된 단말기  

- 사용할 루팅된 단말기의 ABI 확인을 합니다.
~~~
adb shell getprop ro.product.cpu.abi
~~~
- [Frida Release](https://github.com/frida/frida/releases) 에서 단말기 ABI에 해당하고 현재 설치된 Frida 버전과 호환되는 버전의 frida server 버전을 찾아 다운로드합니다.  
   에뮬레이터의 경우, **frida-server-16.1.3-android-x86_64.xz** 처럼 x86, x86_64로 된 frida-server를 다운로드 받으면 됩니다.
- 다운로드 받은 파일의 압축을 해제하고 편의상 frida-server로 이름을 변경합니다.
- adb를 이용해 루팅된 단말기에 frida-server를 설치합니다.
~~~
adb push /frida-server-16.1.3-android-x86_64/frida-server /data/local/tmp/
~~~
- 설치된 frida-server를 실행합니다.
~~~
adb shell "/data/local/tmp/frida-server &"
~~~
- frida-server가 정상적으로 실행 중인지 확인합니다.
~~~
adb shell "ps | grep frida"
~~~
- frida 사용이 가능한지 연결된 단말기에서 실행 중인 앱 목록을 조회합니다.
~~~
frida-ps -U
~~~
- frida server를 이용하여 실행할 스크립트를 로드합니다.   
   이때, 앱이 실행 중이 아니라면 패키지명을 사용하여 연결과 동시에 실행시킬 수도 있습니다. 
    -U : USB로 연결  
    -l {script_path} : 전달된 파일을 로드  
    -n {app_package_name} : 이름의 앱에 붙어서 실행  
    --realm=emulated : 에뮬레이터로 실행  
~~~ 
frida -U -l "/script/hook_getattack.js" -n com.hgo.fridatest
~~~

##### 2. 루팅되지 않은 일반 단말기
Frida Gadget을 이용하면 루팅된 단말기에서도 Frida를 사용할 수 있습니다.
- [Frida Release](https://github.com/frida/frida/releases)에서 사용할 Frida와 동일한 버전의 Frida Gadget을 찾아 다운로드합니다.
- 편의상 다운로드 받은 파일 이름을 **libfrida.so**로 변경합니다.  
- 앱을 실행하여 가장 먼저 실행되는 smali 파일을 찾습니다.
~~~
adb shell "dumpsys window windows | grep -E 'mCurrentFocus|mFocusedApp'"
~~~  
- APKTool을 이용하여 후킹할 앱을 Decompile 합니다.  
- 디컴파일한 앱의 AndroidManifest.xml에 Internet 퍼미션이 없는 경우 추가합니다.  

~~~text
<uses-permission android:name="android.permission.INTERNET"/>
~~~  

- 앱 실행시 가장 먼저 실행되는 smali 파일에 아래 코드를 추가합니다.  
~~~java
const-string v1, "frida"
invoke-static {v1}, Ljava/lang/system;->loadLibrary(Ljava/lang/String;)V
~~~  

   혹은 직접 개발 중이라면 아래와 같은 코드를 직접 onCreate와 같은 시작점에 추가해도 무방합니다.  
~~~java
System.loadLibrary( "frida" );
~~~

- APKTool을 이용하여 다시 리패키징합니다.  
- 리패키징까지 완료된 앱을 단말기에 설치합니다.    
~~~
adb install -r -t -d ./re_game_v1.2.0.apk
~~~  
- 앱을 실행합니다.  
- 실행된 앱이 Frida 연결을 위해 멈춰있을 때 Frida Gadget 연결을 위해 현재 실행 중인 앱 목록을 확인합니다.  
~~~
frida-ps -U
~~~  
- **Gadget** 항목의 PID를 확인하여 연결합니다.  
   -p {PID} : PID를 통해 연결
~~~  
frida -U -l "/script/hook_getattack.js" -p 5482
~~~  
