---
layout: post
title: "Shell의 사용 8 : 그 외 유용한 명령어"
crawlertitle: post_shell_use_8_other_useful
date: 2020-11-15 11:14:59 +0900
categories: shell 
summary: "그 외 Shell에서 유용하게 사용될 명령어관련 내용"
---  
###### 1. Redirection  
&nbsp;앞서 언급된 '출력값 버리기'에 대한 더 자세한 내용으로 프로세스 간 통신의 한 형태로 표준 스트림을 사용자가 지정한 위치로 Redirection할 수 있는 기능입니다. Shell과 같은 대부분의 Command-line Interpreter의 공통적으로 가지고 있습니다.  
<br>
&nbsp;&nbsp;**1.1. 표준 입출력**  
- **_\<command\>_ < _\<input\>_**  
파일 혹은 일반 입력인 _\<input\>_ 에서부터 명령어 _\<command\>_ 로 표준 입력 Redirection시 사용합니다.  
- **_\<command\>_ <\< _\<stream_literal\>_**   
Stream literal(Inline file, 표준 입력으로 전달) _\<stream_literal\>_ 에서부터 명령어 _\<command\>_ 로 표준 입력시 사용합니다.  
- **_\<command\>_ <\<\< _\<string\>_**  
문자열 _\<string\>_ 에서부터 명령어 _\<command\>_ 로 표준 입력시 사용합니다.  
- **_\<command\>_ > _\<output\>_**  
명령어 _\<command\>_ 의 표준 출력을  _\<output\>_ 으로 출력합니다.  
- **_\<command\>_ >\> _\<file\>_**  
명령어 _\<command\>_ 의 표준 출력을 파일 _\<file\>_ 에 이어서 출력합니다.     

    |_\<command\>_ > _\<file\>_|명령어의 표준 출력을 파일에 출력|
    |_\<command\>_ > _\<file\>_ 2>&1|명령어의 표준 출력과 표준 에러 모두 파일로 출력| 
    |_\<command\>_ >\> _\<file\>_|명령어의 표준 출력을 파일에 이어서 출력|
    |_\<command\>_ < _\<file\>_|명령어의 표준 입력으로 파일을 사용|

&nbsp;&nbsp;**1.2. Pipe**  
- **_\<command_1\>_ \| _\<command_2\>_**  
명령어 _\<command_1\>_ 의 표준 출력을 받아 명령어 _\<command_2\>_ 로 입력하는 중간 파일이 명시적으로 존재하지 않아도 함께 실행하여 앞선 명령어의 표준 출력을 | 뒤의 명령어의 표준 입력으로 사용할 수 있습니다. 즉, _\<command_1\>_ 의 표준 출력을 _\<command_2\>_ 의 표준 입력으로 사용합니다. 명령을 실행하는 두 프로그램은 유일한 저장공간인 작업 버퍼에서 각 명령어 처리시 필요한 작업 공간과 병렬로 실행될 수 있습니다. 하지만 실제로는 _\<command_1\>_ 이 실행 완료될 때까지 _\<command_2\>_ 는 실행되지 않습니다. 그리고 각 명령어를 실행할 때 작업 공간뿐만 아니라 중간 결과를 저장하기 위한 충분한 크기의 Scratch file이 필요합니다.   

    |_\<command_1\>_ \| _\<command_2\>_|명령어 A의 표준 출력을 명령어 B의 표준 입력으로 Redirect|

&nbsp;&nbsp;**1.3. 표준 파일 핸들**  
&nbsp;&nbsp;&nbsp;&nbsp;File descriptor 을 **>** 앞, 뒤로 사용하여 Redirection에 사용되는 Stream에 영향을 줍니다.    

|Handle|Name|Description|
|:---:|:---:|:---:|
|0|stdin|표준 입력|
|1|stdout|표준 출력|
|2|stderr|표준 에러|

- **_\<command\>_ _\<file_descriptor\>_> _\<file\>_**
- **_\<command\>_ _\<file_descriptor\>_>\> _\<file\>_**
- **_\<command\>_ _\<file_descriptor\>_>&_\<file_descriptor\>_**   
다른 표준 파일 핸들로 Redirection시에는 앞에 **&**을 사용하여 _\<file\>_ 로 Redirection하는 경우와 구분합니다.  

|_\<command\>_ 1 > _\<file\>_|명령어의 표준 출력을 파일에 출력(위의 command > file과 동일))|
|_\<command\>_ 2 > _\<file\>_|명령어의 표준 에러를 파일에 출력|
|_\<command\>_ 1 >\> _\<file\>_|명령어의 표준 출력을 파일에 이어서 출력(위의 command >> file과 동일)|
|_\<command\>_ 2 >\> _\<file\>_|명령어의 표준 에러를 파일에 이어서 출력|
|_\<command\>_ >\> _\<file\>_ 2>&1|명령어의 표준 출력과 표준 에러를 모두 파일에 이어서 출력|
|_\<command_1\>_ 2>&1 \|  _\<command_2\>_|명령어 A의 표준 출력과 표준 에러를 명령어 B의 표준 입력으로 Redirect|
|_\<command_1\>_ 2>&1|명령어의 표준 에러를 표준 출력으로 Redirect|
|_\<command_1\>_ 1>&2|명령어의 표준 출력을 표준 에러로 Redirect|

###### 2. 환경 변수(Environment Variable)  
&nbsp;Script에서 정의하여 사용하는 내부 변수를 제외한 실행 중인 프로세스가 컴퓨터에서 작동하는 방식에 영향을 주는 동적 변수입니다.   
<br>
&nbsp;&nbsp;**2.1. Shell script**  
- **${env_var}**  
환경 변수도 내부 변수와 동일하게 **$**를 붙여 사용합니다.  

- **export _\<options\>_ _\<env_variable_name\>_=_\<value\>_**  
Shell 변수 _\<env_variable_name\>_ 에 _\<value\>_ 값을 설정합니다. 변수 _\<env_variable_name\>_ 가 유효하지 않거나 명시한 _\<options\>_ 이 잘 못된 경우를 제외하고 성공을 반환합니다.  

    |**Option**|**설명**|
    |:----|:----|
    |-f|Shell Function을 참조합니다.|
    |-n|각 _\<env_variable_name\>_ 에서 export 속성을 제거합니다.|
    |-p|Export된 모든 변수와 함수 목록을 출력합니다.|
    |\--|추가 옵션 처리를 비활성화합니다.|  

- **env _\<options\>_ _\<env_variable_name\>_=_\<value\> _\<run_command\>_**  
환경에서 각 변수 _\<env_variable_name\>_ 에 _\<value\>_ 값을 설정하고 명령 _\<run_command\>_ 을 실행합니다. _\<run_command\>_ 가 없으면 결과 환경을 출력합니다.  

    |**Option**|**설명**|
    |:----|:----|
    |-i <br> --ignore-environment|비어있는 환경에서 시작합니다.|
    |-0 <br> --null|출력 줄의 끝은 Newline이 아닌 NUL로 끝납니다.|
    |-u <br> --unset=_\<name\>_|환경에서 명시된 _\<name\>_ 변수를 제거합니다.|
    |-C <br> --chdir=_\<directory\>_|작업 공간을 _\<directory\>_ 로 변경합니다.|  

- **printenv _\<options\>_ _\<env_variable_names\>_**  
명시한 환경 변수  _\<env_variable_names\>_ 의 값만 출력합니다. 환경 변수 _\<env_variable_names\>_ 를 따로 지정하지 않으면 모든 환경 변수와 그 값을 출력합니다. 

    |**Option**|**설명**|
    |:----|:----|
    |-0 <br> --null|출력 줄의 끝은 Newline이 아닌 NUL로 끝납니다.|  

~~~sh
echo "Locale/Language : "${LC_ALL}
echo "Language : "${LANG}

## LC_ALL 값을 한국_한국어 UTF8로 설정
export LC_ALL=ko_KR.UTF-8

## LANG 값을 한국에서 사용하는 한국어 UTF8로 설정
export LANG=ko_KR.UTF-8

printenv
~~~  

&nbsp;&nbsp;**2.2. Batch file**  
- **%env_variable_name%**  
환경 변수도 내부 변수와 동일하게 **%**로 감싸 사용합니다.  

- **SET _\<env_variable_name\>_=_\<value\>_**  
환경 변수 _\<env_variable_name\>_ 에 _\<value\>_ 값을 설정합니다. _\<value\>_ 값이 비어있으면 해당 변수 자체가 지워지고 _\<env_variable_name\>_ , _\<value\>_ 가 명시되지 않으면 현재 환경 변수를 모두 출력합니다. _\<env_variable_name\>_ 에는 = 기호를 사용할 수 없으며 환경 변수 _\<env_variable_name\>_ 를 찾을 수 없으면 ERRORLEVEL을 1로 설정합니다. 아래는 함께 사용할 수 있는 옵션입니다.

    |**Option**|**설명**|
    |:----|:----|
    |/P|변수의 값을 사용자가 입력한 입력 줄에 설정하도록 허용합니다. 입력 줄을 읽기 전에 지정한 _\<prompt_string\>_ 을 출력하며 해당 값은 비워둘 수 있습니다.|
    |/A|_\<env_variable_name\>_ = _\<value\>_ 를 표현식으로 인식하여 _\<value\>_ 부분에 있는 수식의 결과를 _\<env_variable_name\>_ 에 설정합니다. 표현식에는 아래와 같은 간단한 연산을 지원합니다. 논리 연산자를 사용하면 표현식 문자열을 인용 부호로 묶어야 합니다. 표현식에서 숫자가 아닌 문자열은 환경 변수 문자열로 취급하기 때문에 표현식에서 환경 변수 값을 가져올 때 % 기호를 사용하지 않아도 되며, 그 값은 사용하기 전에 숫자로 변환됩니다. 환경 변수 이름이 지정되었지만 현재 환경에서 정의되지 않았으면 0 값이 사용됩니다. 명령 스크립트 밖의 명령줄에서 SET /A를 실행하면 표현식의 마지막 값이 표시되며 할당 연산자의 왼쪽에 환경 변수 이름이 있어야 합니다. <br> &nbsp;&nbsp;- 그룹 짓기 : ``()`` <br> &nbsp;&nbsp;- 단일 연산자 : ``! ~ -`` <br> &nbsp;&nbsp;- 산술 연산자 : ``* / %`` <br> &nbsp;&nbsp;- 산술 연산자 : ``+ -`` <br> &nbsp;&nbsp;- 논리 이동 :  ``<< >>`` <br> &nbsp;&nbsp;- 비트단위 and : ``&`` <br> &nbsp;&nbsp;- 비트단위 상호 배제 or : ``^`` <br> &nbsp;&nbsp;- 비트단위 or : ``|`` <br> &nbsp;&nbsp;- 할당 : ``= *= /= %= += -=`` <br> &nbsp;&nbsp;- 식 구분 기호 : ``&= ^= |= <<= >>= ,``|  

~~~batch
:: P 로 시작하는 모든 환경 변수를 출력합니다.
SET P

:: USERNAME 출력
ECHO "User Name : "%USERNAME%

SET "PROJECT_DIR=D:\work"
~~~  

&nbsp;&nbsp;**2.3. 자주 사용되는 환경 변수**  

|**Shell script**|**Batch file**|**설명**|
|:----|:----|:----|
|%CD%|\`pwd\`|현재 Directory 경로(문자열), 동적으로 변경되며 경로 끝에 '/'가 없습니다.|
|%~dp0|\`dirname $(realpath $0)\`|Script 실행 위치(현재 Script가 위치하고 있는 경로)| 
||${LC_ALL}|"지역/언어" 정보. LC_*의 값을 재정의합니다.|
||${LANG}|LC_* 값들을 설정하지 않았을 때 적용되는 기본 값|
|%RANDOM%|${RANDOM}|0 ~ 32767 사이 범위(포함)의 임의의 정수를 반환합니다. 상수가 아니며 암호화 키를 생성하는 데 사용해서는 안됩니다.|  

###### 3. Permission  
&nbsp;Script에서 지정한 경로에 위치하는 파일이나 디렉토리에 퍼미션 설정을 합니다.  
<br>
&nbsp;&nbsp;**3.1. Shell script**  
- **chmod _\<options\>_ ${mode} ${path}**
${path}에 위치한 파일이나 디렉토리의 Mode를 ${mode} 값으로 변경합니다. 아래는 옵션에 관한 내용입니다.

    |**Option**|**설명**|
    |:----|:----|
    |-c <br> --changes|--verbose 와 비슷하지만 변경된 내용이 있을 때만 보고합니다.|
    |-f <br> --silent <br> --quiet|대부분의 Error 메세지를 표시하지 않습니다.|
    |-v <br> --verbose|Mode 값을 변경 처리한 모든 파일에 대한 진단을 출력합니다.|
    |--no-preserve-root|’/’를 특별하게 취급하지 않습니다.|
    |--preserve-root|'/'에서 재귀적으로 동작하지 않습니다.|
    |--reference=_\<reile\>_|명시한 ${mode} 값 대신 REILE Mode 값을 사용합니다.|
    |-R <br> --recursive|명시한 ${path}와 그 하위에 있는 파일, 디렉토리에 재귀적으로 변경합니다.|
    
    Mode는 ``[ugoa]*([-+=]([rwxXst]*|[ugo]))+|[-+=][0-7]+`` 형식입니다. 이때 사용되는 문자와 숫자는 아래를 참조하면 됩니다.  

    |**문자**|**숫자**|**8진수**|**설명**|
    |:---:|:---:|:---:|:----|
    |-|0|000|없음. None|
    |x|1|001|실행 <br> 실행 또는 디렉토리 접근에 접근할 수 있습니다.|
    |w|2|010|쓰기 <br> 파일이나 Directory에 쓸 수 있습니다.|
    |r|4|100|읽기 <br> 파일나 Directory를 읽을 수 있습니다.|
    |X|||파일이 Directory이거나 이미 일부 사용자에게 실행 권한이 있을 때만 실행이 가능합니다.|
    |s|||실행시 User나 Group ID를 설정합니다.|
    |t|||제한된 삭제 플래그나 고정 Bit|
    |u|||파일을 소유하고 있는 사용자가 가지고 있는 권한|
    |g|||파일 그룹의 다른 사용자가 가지고 있는 권한|
    |o|||파일 그룹에 없는 다른 사용자가 가지고 있는 권한|

    숫자 Mode는 1~4 자리의 8진수를 사용하며 생략된 모든 숫자는 0으로 취급합니다. 각 사용자에 대한 권한은 아래처럼 사용됩니다.  

    |**rwx**|**8진수**|**설명**|
    |:----|----:|:----|
    |-\--x\--\--\--|100|파일을 소유하고 있는 사용자별 실행|
    |\--w-\--\--\--|200|파일을 소유하고 있는 사용자별 쓰기|
    |-r\--\--\--\--|400|파일을 소유하고 있는 사용자별 읽기|
    |\--wx\--\--\--|300|파일을 소유하고 있는 사용자별 실행, 쓰기|
    |-rw-\--\--\--|600|파일을 소유하고 있는 사용자별 읽기, 쓰기|
    |-rwx\--\--\--|700|파일을 소유하고 있는 사용자별 읽기, 쓰기, 실행|
    |\--\--\--x-\--|010|파일 그룹의 다른 사용자별 실행|
    |-\--\--w\--\--|020|파일 그룹의 다른 사용자별 쓰기|
    |\--\--r-\--\--|040|파일 그룹의 다른 사용자별 읽기|
    |-\--\--wx-\--|030|파일 그룹의 다른 사용자별 쓰기, 실행|
    |\--\--rw\--\--|060|파일 그룹의 다른 사용자별 읽기, 쓰기|
    |\--\--rwx-\--|070|파일 그룹의 다른 사용자별 읽기, 쓰기, 실행|
    |-\--\--\--\--x|001|파일 그룹에 없는 다른 사용자별 실행|
    |\--\--\--\--w-|002|파일 그룹에 없는 다른 사용자별 쓰기|
    |-\--\--\--r\--|004|파일 그룹에 없는 다른 사용자별 읽기|
    |\--\--\--\--wx|003|파일 그룹에 없는 다른 사용자별 쓰기, 실행|
    |-\--\--\--rw-|006|파일 그룹에 없는 다른 사용자별 읽기, 쓰기|
    |-\--\--\--rwx|007|파일 그룹에 없는 다른 사용자별 읽기, 쓰기, 실행|  

~~~sh
cd "work"

## 모든 사용자에게 읽기, 쓰기, 실행 권한 부여(777 = 700 + 070 + 007)
chmod -R 777 "project_01"

## 모든 사용자에게 실행 권한 거부
chmod a-x "project_01/info.txt"

## 소유자를 제외한 사용자에게 읽기, 쓰기 권한 부여
chmod go+rw "project_02/info.txt"
~~~

&nbsp;&nbsp;**3.2. Batch file**  
- **ATTRIB _\<attributes\>_ "%path%" _\<target_option\>_**
%path% 에 위치하는 파일의 특성을 표시하거나 변경합니다. 아래는 파일에 설정할 수 있는 Attribute, 그리고 특성을 지정할 경로에 대한 Option 설명입니다.

    |**Attribute**|**설명**|
    |:----|:----|
    |+|특성을 설정합니다. <br> + 뒤에 설정하고 싶은 특성을 사용하면 됩니다. <br> &nbsp;&nbsp;(예) +R : 읽기 전용 파일 특성 설정|
    |-|특성을 지웁니다. <br> - 뒤에 제거하고 싶은 특성을 사용하면 됩니다. <br> &nbsp;&nbsp;(예) -R : 읽기 전용 파일 특성 제거|
    |R|읽기 전용 파일 특성입니다.|
    |A|보관 파일 특성입니다.|
    |S|시스템 파일 특성입니다.|
    |H|숨김 파일 특성입니다.|
    |O|오프라인 특성입니다.|
    |I|콘텐츠가 인덱싱되지 않은 파일 특성입니다.|
    |X|스크럽 파일 특성이 없습니다.|
    |V|무결성 특성입니다.|
    |P|고정된 특성입니다.|
    |U|고정 해제된 특성입니다.|
    |B|SMR Blob 특성입니다.|  

    |**특성 지정할 경로의 Option**|**설명**|
    |:----|:----|
    |/S|현재 디렉토리와 모든 하위 디렉토리에서 일치하는 파일을 처리합니다.|
    |/D|디렉토리도 처리합니다.|
    |/L|바로 가기 링크의 특성 및 바로 가기 링크의 대상에서 작동합니다.|

~~~batch
CD "work"

:: 읽기 전용, 숨김 파일 권한 설정
ATTRIB +R +H "project_01/info.txt"
~~~  

###### 4. 날짜와 시간  
&nbsp;시스템에 설정된 로컬 시간 기준으로 날짜와 시간을 출력하거나 사용할 수 있습니다.   
<br>
&nbsp;&nbsp;**4.1. Shell script**  
- **date +%\<format_option\>**  
날짜와 시간에 대한 명령어. date의 Format을 따로 설정할 수 있으며, +, -로 원하는 날짜, 시간으로 설정할 수 있습니다. 아래는 자주 사용되는 Format의 옵션입니다.   

    |--------|-------|
    |Option|설명  |
    |:-------:|:------|
    |%D|날짜; mm/dd/yy|
    |%T|시간; hh:MM:ss|
    |%Y|년도; YYYY|
    |%y|년도; yy|
    |%m|월|
    |%d|일|
    |%H|시; 0 ~ 23|
    |%M|분; 0 ~ 59|
    |%S|초; 0 ~ 60| 
    |%Z|TimeZone 약어; KST|
    |%p|오전/오후(AM/PM)|
    |%a|요일 약어; 월 ~ 일, Mon ~ Sun|
    |%A|요일; 월요일 ~ 일요일, Monday ~ Sunday|

    ~~~sh
    echo `date`
    YESTERDAY=`date -d '-1days'`
    echo ${YESTERDAY}
    echo `date +"%Y년 %m월 %d일 %a"`
    ~~~  
- 출력 결과  
~~~text
Sat Jun  6 21:35:56 KST 2020
Fri Jun  5 21:39:34 KST 2020
2020년 06월 06일 Sat
~~~

&nbsp;&nbsp;**4.2. Batch file**  
&nbsp;&nbsp;&nbsp;&nbsp;Shell script와 다르게 날짜와 시간에 대한 명령어가 각각 구분되어 있습니다.    
- **DATE _\<option\>_ %date%**   
YYYY-MM-DD 형태의 날짜이며 필요에 따라 년, 월, 일로 각각 구분할 수 있습니다. 매개변수 없이 DATE만 사용하면 현재 날짜를 출력하며 새로운 날짜를 입력받아 날짜를 갱신합니다. 현재 날짜를 유지하고 싶다면 _\<option\>_ 에 /T 를 사용하여 현재 날짜만 바로 출력합니다. 이후 새로운 날짜에 대한 입력은 받지 않습니다.  
- **TIME _\<option\>_ %time%**   
hh:mm:ss.ff 형태의 시간이며 필요에 따라 시, 분, 초로 각각 구분할 수 있습니다. 매개변수 없이 TIME만 사용하면 현재 시간를 출력하며 새로운 시간를 입력받아 시간를 갱신합니다. 현재 시간를 유지하고 싶다면 _\<option\>_ 에 /T 를 사용하여 현재 시간만 바로 출력합니다. 이후 새로운 시간에 대한 입력은 받지 않습니다.  

    ~~~batch
    :: DATE
    echo Current date : %DATE%
    SET YEAR=%DATE:~0,4%
    SET MONTH=%DATE:~5,2%
    SET DAY=%DATE:~8,2%

    echo Current year : %YEAR%
    echo Current month : %MONTH%
    echo Current day : %DAY%

    echo.

    :: TIME
    echo Current time : %TIME%
    SET HOUR=%TIME:~0,2%
    SET MINUTE=%TIME:~3,2%
    SET SECOND=%TIME:~6,2%
    SET MILLISECOND=%TIME:~9,2%

    echo Current hour : %HOUR%
    echo Current minute : %MINUTE%
    echo Current second : %SECOND%
    echo Current millisecond : %MILLISECOND%
    ~~~   
- 출력 결과   

    ~~~text
    Current date : 2020-06-07
    Current year : 2020
    Current month : 06
    Current day : 07

    Current time : 17:36:53.03
    Current hour : 17
    Current minute : 36
    Current second : 53
    Current millisecond : 03
    ~~~  

###### 5. Shift  
&nbsp;Script에 전달된 매개 변수를 왼쪽으로 이동합니다.  
<br>
&nbsp;&nbsp;**5.1. Shell script**  
- **shift _\<n\>_**  
$N+1, $N+2, ... 위치의 매개 변수를 $1, $2, ... 으로 이동합니다. _\<n\>_ 값이 주어지지 않으면 1로 가정합니다. _\<n\>_ 값이 전달된 매개 변수보다 작거나 $#보다 큰 경우를 제외하고는 성공으로 반환합니다. Batch file과는 다르게 0번째 매개 변수는 이동되지 않습니다.   

    - shift_test.sh  

    ~~~sh
    #!/bin/bash

    echo $0 $1 $2 $3 $4 $5

    shift 0
    echo $0 $1 $2 $3 $4 $5

    shift 2
    echo $0 $1 $2 $3 $4 $5

    ## _\<n\>_ 생략으로 1로 가정
    shift
    echo $0 $1 $2 $3 $4 $5
    ~~~

    - 실행 결과 : ``sh shift_test.sh A B C D E``  

    ~~~text  
    shift_test.sh A B C D E
    shift_test.sh A B C D E
    shift_test.sh C D E
    shift_test.sh D E
    ~~~  

&nbsp;&nbsp;**5.2. Batch file**  
- **SHIFT /_\<n\>_**  
Batch file에 전달된 매개 변수를 이동합니다. /_\<n\>_ 을 사용하여 _\<n\>_ 번째 매개 변수부터 이동하도록 지정할 수 있습니다. 매개 변수의 이동은 N번째 매개 변수 값을 N-1번째 매개 변수 위치에 복사하는 형태로 %1 값을 %0에, %2 값을 %1에 복사하여 %0 ~ %9의 매개 변수의 값을 변경합니다. 매개 변수는 10개 이상 사용할 수 있으며 10번째 매개 변수부터는 SHIFT 한 번에 한 번씩 %9 위치로 값이 이동됩니다. %* 매개 변수는 SHIFT 명령어에 영향을 주지 않으며 SHIFT를 사용해서 이미 이동된 값은 복구를 할 수 없습니다.

    - shift_test.bat

    ~~~batch  
    @ECHO OFF

    :: 전달된 매개 변수 그대로 출력
    ECHO %0 %1 %2 %3 %4

    :: %0(<-%1) %1(<-%2) %2(<-%3) %3(<-%4) %4(<-%5)
    SHIFT
    ECHO %0 %1 %2 %3 %4

    :: %0 %1 %2(<-%3) %3(<-%4) %4(<-%5)
    SHIFT /2
    ECHO %0 %1 %2 %3 %4

    SHIFT
    ECHO %0 %1 %2 %3 %4
    ~~~  

    - 실행 결과 : ``CALL shift_test.bat A B C D E``  

    ~~~text
    shift_test.bat A B C D E
    A B C D E
    A B D E
    B D E
    ~~~   

###### 6. 환경 변수의 지역화  
&nbsp;Script에서 환경 변수를 지역화를 설정 및 해제할 수 있습니다.  
<br>
&nbsp;&nbsp;**6.1. Shell script**  
&nbsp;&nbsp;&nbsp;&nbsp;Shell script에서는 정의 및 사용하는 변수가 Script 종료된 이후에도 유지되지 않습니다.  기본적으로 지역화가 이미 설정된 상태입니다.   
<br>
&nbsp;&nbsp;**6.2. Batch file**  
- **SETLOCAL _\<argument\>_ ~ ENDLOCAL**  
**SETLOCAL**부터 환경 변수의 지역화가 설정되어 **ENDLOCAL** 전까지 유지됩니다. **SETLOCAL** 이후 처리되는 환경 변경은 해당 Script에서만 유효하며 이전 설정으로 복구하려면 **ENDLOCAL**을 사용해야 합니다. 따로 **ENDLOCAL**을 명시하지 않아도 Script 끝에 도달하면 암시적인 **ENDLOCAL**이 실행됩니다. **ENDLOCAL** 명령 실행 이후에 처리된 환경 변화는 Script 외부에도 적용되며 이전 설정은 Script가 끝난 후에도 복구되지 않습니다. **SETLOCAL**은 동일한 환경 변수에 여러 값을 설정할 수 있고 **ERRORLEVEL** 값을 설정합니다. 아래는 명령 확장을 위한 **SETLOCAL**의 인수입니다.

|**인수**|**설명**|
|:----|:----|
|ENABLEEXTENSIONS <br> DISABLEEXTENSIONS|**SETLOCAL** 명령 실행 전 설정과 상관없이 **ENDLOCAL** 명령 실행 전까지 '명령 확장 기능'이 활성화/비활성화됩니다. <br> 기본적으로 활성화되어있으며 비활성화할 필요가 거의 없습니다.|
|ENABLEDELAYEDEXPANSION <br> DISABLEDELAYEDEXPANSION|**SETLOCAL** 명령 실행 전 설정과 상관없이 **ENDLOCAL** 명령 실행 전까지 '지연된 환경 변수 확장 기능'이 활성화/비활성화됩니다.<br>Batch file의 실행을 위해 읽을 때 환경변수가 확장되는 데 ``ENABLEDELAYEDEXPANSION``이 활성화된 경우 읽을 때가 아니라 실행될 때 환경변수가 확장되며 **%** 대신 **!**를 사용하여 환경변수 확장 기능으로 실행 중에 환경변수 값을 변경할 수 있습니다.|

~~~batch
:: 변수에 여러 번 값 설정  
SETLOCAL ENABLEDELAYEDEXPANSION
SET TARGET_FILE_PATH=%1
SET INDEX=0

IF EXIST %TARGET_FILE_PATH% (
    FOR /F "delims=" %%L IN ( %TARGET_FILE_PATH% ) DO (
        :: 5줄만 읽습니다.
        IF !INDEX! LSS 5 (
            ECHO "[ "!INDEX!" ] "%%L
        )

        SET /a INDEX+=1
    )
)
ENDLOCAL
~~~  

###### 7. Shell Option
&nbsp;Shell Option 설정 및 해제합니다.  
<br>
&nbsp;&nbsp;**7.1. Shell script**  
- **shopt _\<options\>_ _\<option_names\>_**
명시된 Shell Option _\<option_names\>_ 을 설정 및 해제합니다. _\<options\>_ 와 _\<option_names\>_ 없이 명령어만 사용하면 모든 Shell option을 각각의 설정 여부와 함께 출력합니다. _\<option_names\>_ 가 활성화된 경우 성공, 잘못된 옵션이거나 _\<option_names\>_ 가 비활성화된 경우 실패를 반환합니다. 아래는 함께 사용되는 옵션입니다.   

|**Option**|**설명**|
|:----|:----|
|-o|_\<option_names\>_ 를 ``set -o``와 함께 사용해 정의된 것으로 제한합니다.|
|-p|각 Shell Option을 상태와 함께 출력합니다.|
|-q|출력을 제한합니다.|
|-s|각 _\<option_names\>_ 을 활성화(설정)합니다.|
|-u|각 _\<option_names\>_ 을 비활성화(해제)합니다.|  

아래는 Shell option입니다.   

|**Shell Option**|**설명**|
|:----|:----|
|extglob|설정된 경우, 확장된 Pattern matching 기능이 활성화됩니다.|
|extdebug|Shell 호출이나 Shell 시작 파일에서 설정된 경우, --debugger 옵션과 동일한 Debugger Profile을 실행하도록 정렬합니다.|
|그 외|autocd, cdable_vars, cdspell, checkhash, checkjobs, checkwinsize, cmdhist, compat31, compat32, compat40, compat41, compat42, compat43, complete_fullquote, direxpand, dirspell, dotglob, execfail, expand_aliases, extquote, failglob, force_fignore, globasciiranges, globstar, gnu_errfmt, histappend, histreedit, histverify, hostcomplete, huponexit, inherit_errexit, interactive_comments, lastpipe, lithist, login_shell, mailwarn, no_empty_cmd_completion, nocaseglob, nocasematch, nullglob, progcomp, promptvars, restricted_shell, xpg_echo|

~~~sh
## Shell option 전체 출력
shopt

shopt -s extglob

shopt -u extglob
~~~  

###### 8. 표현식 연산
&nbsp;표현식을 연산(평가)하고 그 결과를 출력합니다.   
<br>
&nbsp;&nbsp;**8.1. Shell script**   
- **expr _\<option\>_**
- **expr _\<expression\>_**  
표현식 _\<expression\>_ 을 평가한 결과를 표준 출력으로 출력합니다.   
표현식 _\<expression\>_ 의 각 Token은 별도의 전달인자여야하고 피연산자는 정수 또는 문자열입니다.  
정수는 하나 이상의 숫자로 구성되며 숫자 앞에  '-'을 선택적으로 사용할 수 있습니다. **expr**의 피연산자는 적용되는 연산에 따라 정수나 문자열로 변환합니다.  
문자열은 **expr** 자체에서 **"**(따옴표)를 사용하지 않지만 공백과 같은 특별한 의미를 가진 문자를 위해 사용될 수 있습니다. 그러나 **"**(따옴표) 사용과 관계없이 문자열 피연산자는 괄호나 + 와 같은 **expr**의 연산자가 되면 안되므로 Shell에서 임의의 문자열 ${string}에 **"**(따옴표)를 사용하는 것만으로는 **expr**에 안전하게 전달할 수 없습니다. 음의 정수나 '-'으로 시작하는 문자열은 옵션으로 잘 못 해석될 수 있으므로 **expr**의 첫 번째 전달인자가 될 수 없지만 괄호를 사용한다면 가능합니다. 또한, 휴대용 스크립트에서는 정수 형태인 문자열 피연산자가 사용될 수 없지만 전달 인자 앞에 공백을 삽입한다면 가능합니다.   
연산자는 Infix Symbol이나 접두사 키워드가 사용 가능합니다. 괄호는 일반적인 방법의 그룹화에 사용되지만 Shell에서 괄호나 연산자가 처리되는 것을 피하기 위해 **"**(따옴표)를 사용해야 합니다.  표현식 _\<expression\>_ 에 정규표현식을 사용할 수도 있습니다. 주로 문자열에 사용되며 고정된 패턴에 일치되는 지를 확인합니다.   
아래는 **expr**의 실행시 반환 값입니다.  

    |**Exit status**|**설명**|
    |:---:|:----|
    |0|_\<expression\>_ 가 null도 0도 아님|
    |1|_\<expression\>_ 가 null이나 0|
    |2|_\<expression\>_ 구문 자체에 문제가 있어 유효하지 않음|
    |3|_\<expression\>_ 에러가 발생한 경우(예 : 연산 오버플로우)|  

&nbsp;&nbsp;&nbsp;**8.1.1. String expressions**  
&nbsp;&nbsp;&nbsp;&nbsp;문자열 표현식으로 **expr**은 Pattern Matching과 다른 문자열 연산자를 제공합니다. 이는 숫자 연산자나 관계 연산자보다 우선순위가 높습니다. 키워드를 문자열로 해석하도록 하기 위해서는 **"**(따옴표)를 사용해야 합니다.  

- **${string} : \<regex\>**  
인수는 문자열로 변환되고 두 번째 인자인 \<regex\>는 **^**가 암시적으로 미리 입력된 정규 표현식으로 인식되어 첫 번째 인자와 일치하는 지 비교합니다.  
표현식이 일치할 때, \<regex\>에서 \( \)를 사용하고 있으면 하위 표현식에서 일치하는 문자열을 반환하고 그렇지 않으면 일치하는 문자의 수를 반환합니다.
일치하거나 정규표현식에서 \( \)를 사용하는 경우 표현식은 반환합니다.  
실패할 때, **:** 연산자와 \<regex\>에서 \( \)를 사용하고 있으면 null, 그렇지 않으면 0을 반환합니다. 첫 번째 \( \)만 반환 값에 관련이 있고 그 외 사용되는 \( \)는 정규표현식 연산자를 위한 그룹화하기 위해 사용됩니다. 정규표현식에서 **+**, **?**, **\|**는 각각 하나 이상, 0 또는 하나, 별개의 대안과 일치하는 연산자입니다.  
<br>
- **match ${string} \<regex\>**  
Pattern Matching을 위한 대안으로 ${string} : \<regex\> 와 동일합니다.  
<br>
- **substr ${string} \<position\> \<length\>**  
문자열 ${string}에서 \<position\> 번째 문자부터 길이 \<length\>만큼 잘라 반환합니다. \<position\>이나 \<length\>가 음수이거나 0이거나 숫자가 아니면 null을 반환합니다.  
<br>
- **index ${string} \<charset\>**   
문자열 ${string}에서 \<charset\>을 찾은 첫 번째 위치를 반환합니다. \<charset\>을 찾지 못하면 0을 반환합니다.  
<br>
- **length ${string}**    
문자열 ${string}의 길이를 반환합니다.  
<br>
- **+ \<token\>**    
'match' 같은 키워드나 '/' 같은 연산자인 경우에도 \<token\>을 문자열로 해석합니다. 이를 통해 ``expr length + "$x"`` 나 ``expr + "$x" : '.*/\(.\)'`` 를 테스트할 수 있고 $x 의 값이 / 이나 index이여도 정상적으로 동작합니다. Shell에 따라 ``" $token" : ' \(.*\)'`` 대신 ``+ "$token"``을 사용해야 할 수도 있습니다.  
 
~~~sh
TEMP=

file="info.txt"
while IFS=',' read -r line || [ -n $line ]
do
    ##Skip empty line
    if [ -z "$line" ]
    then
        continue
    fi

    echo ${line}

    ## Carriage return 제거 
    TEMP=`expr "${line}" | tr -d '\15\32'`
    echo "> Remove Carriage return : "${TEMP}
    
    ## 소문자로 변경
    TEMP=`expr "${TEMP}" | tr '[:upper:]' '[:lower:]'`
    echo "> To Lower : "${TEMP}
    
    ## 문자열 치환 : 확장자 제거
    TEMP=`expr "${TEMP}" | sed -e "s/*.txt//g"`
    echo "> Replace .txt to empty : "${TEMP}
    
    ## 문자열 치환
    TEMP=`expr "${TEMP}" | sed -e "s/column*/row/g"`
    echo "> Replace column to row : "${TEMP}
done <"$file"
~~~  

&nbsp;&nbsp;&nbsp;**8.1.2. Numeric expressions**  
&nbsp;&nbsp;&nbsp;&nbsp;**expr**는 우선순위를 높이기 위해 숫자 연산자를 제공합니다. 숫자 연산자는 문자열 연산자보다 우선순위가 낮고 관계 연산자보다는 높습니다.  

- **${argument_0} + ${argument_1}**  
덧셈 연산자로 인자 ${argument_0}과 ${argument_1}을 정수로 변환하며 연산할 수 없는 경우 에러가 발생합니다.  
<br>
- **${argument_0} - ${argument_1}**  
뺄셈 연산자로 인자 ${argument_0}과 ${argument_1}을 정수로 변환하며 연산할 수 없는 경우 에러가 발생합니다.  
<br>
- **${argument_0} * ${argument_1}**  
곱셈 연산자로, 인자 ${argument_0}과 ${argument_1}을 정수로 변환하며 연산할 수 없는 경우 에러가 발생합니다.  
<br>
- **${argument_0} / ${argument_1}**  
나눗셈 연산자로, 인자 ${argument_0}과 ${argument_1}을 정수로 변환하며 연산할 수 없는 경우 에러가 발생합니다.  
<br>
- **${argument_0} % ${argument_1}**  
나머지 연산자로, ${argument_0}에서 ${argument_1}을 나누고 남은 값을 반환합니다. 인자 ${argument_0}과 ${argument_1}을 정수로 변환하며 연산할 수 없는 경우 에러가 발생합니다.  
    
&nbsp;&nbsp;&nbsp;**8.1.3. Relations for expr**  
&nbsp;&nbsp;&nbsp;&nbsp;**expr**는 일반적인 논리 관계 연산자를 제공합니다. 이 연산자는 숫자, 문자열 연산자보다 우선순위가 낮습니다.  

- **${argument_0} | ${argument_1}**  
첫 번째 인자인 ${argument_0}이 null도 0도 아니면 ${argument_0}, 그렇지 않고 두 번째 인자인 ${argument_1}이 null도 0도 아니면 ${argument_1}, 둘 다 아니면 0을 반환합니다. 첫 번째 인자인 ${argument_0}이 null도 0도 아니면 두 번째 인자는 처리하지 않습니다.  
<br>
- **${argument_0} & ${argument_1}**  
인자가 null이나 0이 아니면 ${argument_0}을 반환하고 그렇지 않으면 0을 반환합니다. 첫 번째 인자가 null이거나 0이면 두 번째 인자는 처리하지 않습니다.  
<br>
- **${argument_0} < ${argument_1}**  
인자를 비교하여 ${argument_0}보다 ${argument_1}이 크면 1 그렇지 않으면 0을 반환합니다. **expr**은 두 인자를 정수로 변환하여 비교하는데 변환에 실패하면 지정된 ${LC_COLLATE} Locale에 따라 사전순으로 비교합니다.  
<br>
- **${argument_0} <= ${argument_1}**  
인자를 비교하여 ${argument_0}보다 ${argument_1}이 크거나 같으면 1 그렇지 않으면 0을 반환합니다. **expr**은 두 인자를 정수로 변환하여 비교하는데 변환에 실패하면 지정된 ${LC_COLLATE} Locale에 따라 사전순으로 비교합니다.  
<br>
- **${argument_0} = ${argument_1}**
- **${argument_0} == ${argument_1}**  
인자를 비교하여 ${argument_0}과 ${argument_1}이 같으면 1 그렇지 않으면 0을 반환합니다. **expr**은 두 인자를 정수로 변환하여 비교하는데 변환에 실패하면 지정된 ${LC_COLLATE} Locale에 따라 사전순으로 비교합니다.  
<br>
- **${argument_0} != ${argument_1}**  
인자를 비교하여 ${argument_0}과 ${argument_1}이 같지 않으면 1 그렇지 않으면 0을 반환합니다. **expr**은 두 인자를 정수로 변환하여 비교하는데 변환에 실패하면 지정된 ${LC_COLLATE} Locale에 따라 사전순으로 비교합니다.  
<br>
- **${argument_0} >= ${argument_1}**  
인자를 비교하여 ${argument_1}이 ${argument_0}보다 작거나 같으면 1 그렇지 않으면 0을 반환합니다. **expr**은 두 인자를 정수로 변환하여 비교하는데 변환에 실패하면 지정된 ${LC_COLLATE} Locale에 따라 사전순으로 비교합니다.  
<br>
- **${argument_0} > ${argument_1}**  
인자를 비교하여 ${argument_1}이 ${argument_0}보다 작으면 1 그렇지 않으면 0을 반환합니다. **expr**은 두 인자를 정수로 변환하여 비교하는데 변환에 실패하면 지정된 ${LC_COLLATE} Locale에 따라 사전순으로 비교합니다.  
<br>

###### 9. Job Control  
&nbsp;선택에 의해 프로세스를 실행, 일시 중지, 재개할 수 있습니다.
<br>
&nbsp;&nbsp;**9.1. Shell script**
- **fg \<job_spec\>**  
Job을 Foreground로 이동합니다. \<job_spec\>로 식별된 Job을 Foreground에 배치하여 현재 Job으로 만듭니다. \<job_spec\>이 존재하지 않으면 현재 작업에 대한 Shell의 개념이 사용됩니다. 종료 상태값은 성공시 Foreground에 배치된 명령 상태값, 오류가 발생하면 실패를 반환합니다.  
<br>
- **bg \<job_specs\>**  
- **& \<job_specs\>**  
Job을 Background로 이동합니다. \<job_specs\>로 식별된 여러 Job들을 각각 Background에 배치합니다. \<job_specs\>이 존재하지 않으면 현재 작업에 대한 Shell의 개념이 사용됩니다. Job control이 비활성화 상태이거나 오류가 발생하면 실패, 그렇지 않으면 성공을 반환합니다.  
<br>
- **jobs _\<option\>_ _\<job_specs\>_**    
현재 활성화된 Job들의 상태를 출력합니다. _\<job_specs\>_ 이 지정된 경우 지정된 Job들만 출력되고 옵션이 없으면 모든 활성 상태의 Job들이 출력됩니다. 아래는 함께 사용할 수 있는 옵션입니다. 잘못된 옵션이 사용되거나 오류가 발생하면 실패, 그렇지 않으면 성공입니다.  

    |**Option**|**설명**|
    |:----|:----|
    |-l|일반 정보 외에 Process ID를 나열합니다.|
    |-n|마지막 알림 이후 상태가 변경된 Process들만 나열합니다.| 
    |-p|Process ID만 나열합니다.|
    |-r|현재 실행 중인 Job으로만 제한하여 출력합니다.|
    |-s|현재 정지된 Job으로만 제한하여 출력합니다.|

- **jobs -x \<command\> _\<command_arguments\>_**  
현재 활성화된 Job들의 상태를 출력합니다. -x가 제공되면 _\<command_arguments\>_ 에 표시되는 모든 Job이 해당 Job의 Process Group Leader의 Process ID로 교체된 후에 \<command\>가 실행됩니다. 잘못된 옵션이 사용되거나 오류가 발생하면 실패, 그렇지 않으면 성공입니다. -x가 사용되는 경우 \<command\>의 종료 상태가 반환됩니다.  
<br>
- **kill _\<option\> pid \| \<job_specs\>**   
- **kill -l \<sig_spec\>**  
Job에 Signal을 보냅니다. SIGSPEC이나 SIGNUM인 Signal을 PID나 JOBSPEC으로 식별된 프로세스로 보냅니다. SIGSPEC, SIGNUM이 둘 다 존재하지 않으면 SIGTERM으로 가정합니다. Process ID 대신 Job ID를 대신 사용할 수 있고 생성할 수 있는 Process 제한에 도달하면 Process를 중지할 수 있습니다. 이 두 가지 이유로 Shell에 내장되어 있습니다. 잘못된 옵션이 사용되거나 오류가 발생하면 실패, 그렇지 않으면 성공입니다. 아래는 함께 사용할 수 있는 옵션입니다.  

    |**Option**|**설명**|
    |:----|:----|
    |-s \<sig_name\>|\<sig_name\>은 Singal의 이름입니다.|
    |-n \<sig_num\>|\<sig_num\>은 Signal의 번호입니다.|
    |-l<br>-L|Signal 이름의 목록입니다. -l 뒤에 인자가 있는 경우 나열되어야 하는 Signal 이름의 번호로 가정합니다.|  

<br>
- **wait _\<option\>_ \<ids\>**  
Job이 완료되어 종료 상태를 반환할 때까지 기다립니다. Process ID나 Job Spec일 수도 있는 ID로 식별된 각 Process를 기다렸다가 종료 상태를 보고합니다. ID가 주어지지 않은 경우 현재 활성화된 모든 Child Process를 기다리며 0을 반환합니다. ID가 Job Spec인 경우 Job의 Pipeline에 있는 모든 Process를 기다립니다. 잘못된 옵션이나 잘못된 ID가 사용된 경우 실패, 그렇지 않으면 마지막 ID의 상태를 반환합니다. _\<option\>_ 에 -n 을 사용한 경우 다음 Job이 종료할 때까지 기다렸다가 종료 상태를 반환합니다.  
<br>
- **disown _\<option\>_ \<job_specs\> | \<pids\>**  
현재 Shell에서 Job들을 제거합니다. 활성화된 Job Table에서 각 JOBSPEC 인자를 제거합니다. \<job_specs\>이 없으면 Shell에서 현재 Job의 개념을 사용합니다. 잘못된 옵션이나 \<job_specs\>이 사용되는 경우 실패, 그렇지 않은 경우 성공입니다. 아래는 _\<option\>_ 에 사용 가능한 옵션입니다.  

    |**Option**|**설명**|
    |:----|:----|
    |-a|\<job_specs\>이 제공되지 않은 경우 모든 Job을 제거합니다.|
    |-h|Shell에서 SIGHUP을 수신할 경우 Job에 SIGHUP을 보내지 않도록 각 \<job_spec\>에 표시합니다.|
    |-r|실행 중인 Job들만 제거합니다.|

<br>
- **suspend _\<option\>_**  
Shell 실행을 일시 중지합니다. Shell이 SIGCONT 신호를 받을 때까지 실행을 일시 중지합니다. _\<option\>_ 에 -f 옵션을 지정하여 강제하지 않는 한 Login shell은 일시 중지할 수 없습니다. -f 옵션은 Login shell조차 강제로 일시 중지합니다. Job control이 비활성화 상태이거나 오류가 발생하면 실패, 그렇지 않으면 성공을 반환합니다.  

&nbsp;&nbsp;**9.2. Batch file**  
- **START "%TITLE%" "%PATH%" _\<options\>_ \<command\> _\<command_arguments\>_**  
지정한 프로그램이나 명령을 실행할 수 있도록 별도의 창을 시작합니다. Shell script의 bg와 비슷하게 Background process를 실행합니다. 이때, 창 제목 표시줄에 출력될 제목은 "%TITLE%"을 사용하고 창 시작 Directory는 "%PATH%"을 사용합니다.  
매개변수 _|\<command_arguments\>_ 를 전달하여 명령 또는 프로그램인 \<command\>를 실행합니다. 이때, \<command\>가 명령인지 프로그램인지에 따라 실행 형태가 다릅니다. \<command\>가 내부 cmd 명령 또는 배치 파일이면 명령 처리기에서 /K 을 사용하여 **cmd.exe**를 실행하기 때문에 명령 실행 후에도 창이 남게 됩니다. \<command\>가 프로그램이면 창 모드 응용 프로그램이나 콘솔 응용 프로그램으로 실행됩니다.  
명령 확장을 사용하면 파일 이름만으로 명령줄을 통한 외부 명령 호출이나 START 명령이 가능합니다. 일반적으로 지정한 파일의 확장자와 연결된 응용프로그램을 실행합니다.  
확장자나 경로 한정자 없이 첫 토큰이 CMD인 명령줄을 실행할 때는 CMD를 COMSPEC 변수의 값으로 바꾸며 따라서 최소한의 경우 임의의 CMD.EXE 버전이 선택되는 것을 막을 수 있습니다. 첫 토큰이 확장자를 가지지 않는 명령줄을 실행할 때 CMD.EXE는 어떤 확장자를 어떤 순서로 찾을 것인지 결정하기 위해 PATHEXT 환경 변수 값(기본값 : .COM;.EXE;.BAT;.CMD)을 사용합니다. 명령을 실행할 때는 확장자가 같지 않으면 확장자가 없는 이름이 디렉터리 이름과 같은 지 찾아보고 만약 있으면 START명령이 그 경로에서 탐색기를 시작합니다.  
아래는 함께 사용되는 옵션입니다.  

    |**Option**|**설명**|
    |:----|:----|
    |B|새 창을 만들지 않고 응용 프로그램을 시작합니다. 응용 프로그램에서 ^C 처리를 무시합니다. 응용 프로그램이 ^C 처리를 활성화하지 않는 한 ^Break로만 응용 프로그램을 인터럽트할 수 있습니다.|
    |I|현재 환경이 아닌 새 환경을 원래의 환경 값으로 cmd.exe에 전달합니다.|
    |MIN|창을 최소화하여 시작합니다.|
    |MAX|전체 화면을 표시하며 시작합니다.|
    |NODE <NUMA \<node\>>|기본 NUMA(Non-Uniform Memory Architecture) 노드를 10진수 정수로 지정하고 NUMA 시스템의 메모리 위치를 활용하는 방식으로 프로세스를 생성할 수 있습니다.|
    |AFFINITY \<hex_priority\>|프로세서 우선순위를 16진수로 지정합니다. 프로세스는 현재 실행 중인 처리기로 제한됩니다. /AFFINITY와 /NODE가 조합되면 우선순위는 다르게 처리됩니다. NUMA 노드의 처리기 Flag가 곧바로 전환되어 0비트에서 시작하는 것처럼 우선순위를 지정하십시오. 프로세스는 지정한 우선순위와 NUMA 노드 간에 공통이면서 현재 실행 중인 처리기로 제한됩니다. 공통 처리기가 없으면 프로세스는 지정한 NUMA 노드에서 실행 중인 처리기로 제한됩니다.|
    |WAIT|응용 프로그램을 시작하고 끝날 때까지 기다립니다.|
    |**시작 메모리 영역 지정**|**설명**|
    ||64Bit Platform에서 사용할 수 없으며 ``SEPARATE``와 ``SHARED`` 둘 중 하나만 지정할 수 있습니다.|
    |SEPARATE|16비트 Windows 프로그램을 별도의 메모리 영역에서 시작합니다.|
    |SHARED|16비트 Windows 프로그램을 공유 메모리 영역에서 시작합니다.|
    |**우선순위**|**설명**|
    |LOW|응용 프로그램을 IDLE 우선 순위 클래스에서 시작합니다.|
    |NORMAL|응용 프로그램을 NORMAL 우선 순위 클래스에서 시작합니다.|
    |HIGH|응용 프로그램을 HIGH 우선 순위 클래스에서 시작합니다.|
    |REALTIME|응용 프로그램을 REALTIME 우선 순위 클래스에서 시작합니다.|
    |ABOVENORMAL|응용 프로그램을 ABOVENORMAL 우선 순위 클래스에서 시작합니다.|
    |BELOWNORMAL|응용 프로그램을 BELOWNORMAL 우선 순위 클래스에서 시작합니다.|un