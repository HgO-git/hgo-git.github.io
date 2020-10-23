---
layout: post
title: "Shell의 사용 3 : 반복문"
crawlertitle: post_shell_use_3_looping_statement
date: 2020-07-05 16:16:00 +0900
categories: shell
summary: "Shell에서 반복문.<br>
제어문 중 반복문에 대한 대한 내용입니다."
---
###### 1. While    
&nbsp;주어진 조건이 true인 경우 반복되는 구문입니다.  
<br>
&nbsp;**1.1. Shell script**   
- **while [ \<condition\> ] do ~ done**
- **while [ \<condition\> ]; do ~ done**  
한 줄로 처리하고 싶을 때는 while과 조건 바로 뒤에 **;**를 추가합니다.

~~~sh
INDEX=0
while [ ${INDEX} -lt 5  ]
do
    echo "while : "${INDEX}
    INDEX=$(($INDEX+1))
done
~~~  

&nbsp;**1.2. Batch file**  
&nbsp;&nbsp;Batch file에는 WHILE문이 따로 없어서 GOTO문을 사용하여 비슷하게 구현할 수 있습니다.
- **:WHILE ~ IF \<condition\> GOTO QUIT ~ GOTO WHILE ~ :QUIT**  

~~~batch
SETLOCAL ENABLEDELAYEDEXPANSION
SET INDEX=

:: WHILE문 시작 
:WHILE
ECHO "WHILE : "!INDEX!
SET /a INDEX+=1

::해당 조건을 만족하면 반복합니다. 
IF !INDEX! LSS 5 (
    GOTO WHILE
) ELSE (
    GOTO QUIT
)

:: WHILE문 종료
:QUIT

ENDLOCAL
~~~  

###### 2. For  
&nbsp;주어진 조건이 true인 경우 또는 주어진 배열까지 반복되는 구문입니다.  
<br>
&nbsp;**2.1. Shell script**   
- **for _${variable}_ in _${loop_array}_ do ~ done**  
- **for (( \<initializer\>; \<condition\>; \<counting_expression\> ))**  
초기화한 변수를 특정 조건까지 반복합니다.  

~~~sh
ARRAY_UNDER_5=(0 1 2 3 4)
for value in ${ARRAY_UNDER_5}
do
    echo "for : "${value}
done

for (( index=0; index<5; i++ ))
do
    echo "for : "${index}
done
~~~

&nbsp;**2.2. Batch file**    
- **FOR _\<option\>_ %%\<element_variable_c\> IN ( _%loop_array%_ ) DO ~**  
주어진 배열만큼 반복합니다. 이때, 배열의 값에 대한 변수는 **%%**를 사용하며 변수명은 다른 변수와 중복되지 않아야 합니다. 이때, 사용되는 \<element_variable_c\> 는 한 글자의 문자입니다. _%loop_array%_ 에는 하나 이상의 파일들, 배열, 문자열 그리고 명령어도 사용이 가능합니다.  

    - _\<option\>_  

    |**/F Keyword**|**설명**|
    |:----|:----|
    |**Option**|**설명**|
    |:----|:----|
    |/D|_%loop_array%_ 에 대표 문자가 있으면 파일 이름 대신 디렉터리 이름과 일치하도록 지정합니다.|
    |/R|전달된 경로를 Root로 하여 Directory Tree를 따라 내려가며 Tree의 각 Directory에서 FOR 구문을 실행합니다.<br>/R 뒤에 경로가 지정되지 않으면 현재 Directory가 사용되고 _%loop_array%_ 에 마침표( . )가 사용되면 Directory Tree만 나열합니다.|
    |/L|_%loop_array%_ 기준으로 단계별로 증가/감소하는 시작부터 끝까지의 일련의 숫자를 나열합니다.<br>예 : (1,1,5) -> 1 2 3 4 5<br>(5,-1,1) -> 5 4 3 2 1|
    |/F|하나 이상의 파일들에 대해 조건부로 각 항목에 대한 명령을 수행합니다.<br>이때, 아래와 같은 키워드로 열기 또는 읽기 작업을 진행 중인 파일에 대한 구문 분석을 진행합니다.|

    - _\<option\>_ 에 **/F** 사용시 함께 사용할 수 있는 키워드  

    |**/F Keyword**|**설명**|
    |:----|:----|
    |eol=c|행 끝 설명 문자를 하나만 지정합니다.|
    |skip=n|파일의 시작 부분에서 무시할 행의 개수를 지정합니다.|
    |delims=_\<delimiter\>_|_\<delimiter\>_ 로 구분 문자 집합을 지정합니다.  이것은 공백 또는 탭에 대한 기본 구분 문자 집합을 바꿉니다.|
    |tokens=_\<token_list\>_|각 줄에서 어떤 토큰이 각 반복에 대한 For 구문으로 전달될지를 지정합니다. 이 작업은 추가 변수 이름이 할당되도록 됩니다.|
    |usebackq|역 따옴표( ` ) 내의 문자열을 명령으로 처리하며, 작은따옴표( ' )는 문자열 명령어로 큰따옴표( " )는 파일 이름을 나타내도록 사용합니다.|

    - FOR의 대체 변수 %%\<element_variable_c\>에서 사용할 수 있는 확장 기능  

    |**%%\<element_variable_c\> 변수 확장**|**설명**|
    |:----|:----|
    |%~\<element_variable_c\>|따옴표( " )를 제거하는 %\<element_variable_c\>을 확장합니다.|
    |%~f\<element_variable_c\>|%\<element_variable_c\>을 정규화된 경로 이름으로 확장합니다.|
    |%~d\<element_variable_c\>|%\<element_variable_c\>을 드라이브 문자로만 확장합니다.|
    |%~p\<element_variable_c\>|%\<element_variable_c\>을 경로로만 확장합니다.|
    |%~n\<element_variable_c\>|%\<element_variable_c\>을 파일 이름으로만 확장합니다.|
    |%~x\<element_variable_c\>|%\<element_variable_c\>을 파일 확장명으로만 확장합니다.|
    |%~s\<element_variable_c\>|확장된 경로가 짧은 이름만 가지고 있습니다.|
    |%~a\<element_variable_c\>|%\<element_variable_c\>이 파일의 파일 속성으로만 확장합니다.|
    |%~t\<element_variable_c\>|%\<element_variable_c\>을 파일의 날짜/시간으로만 확장합니다.|
    |%~z\<element_variable_c\>|%\<element_variable_c\>을 파일 크기로만 확장합니다.|
    |%~$PATH:\<element_variable_c\>|PATH 환경 변수 목록에 있는 Directory를 찾고 %\<element_variable_c\>을 처음으로 찾은 정규화된 이름으로 확장합니다.<br>환경 변수 이름이 정의되지 않았거나 찾기에서 파일을 찾지 못하면 이 구문에서 빈 문자열로 확장합니다.|
    |%~dp\<element_variable_c\>|%\<element_variable_c\>을 Drive 문자와 경로로만 확장합니다.|
    %~nx\<element_variable_c\>|%\<element_variable_c\>을 파일 이름과 확장명으로만 확장합니다.|
    %~fs\<element_variable_c\>|%\<element_variable_c\>을 짧은 이름을 가진 전체 경로 이름으로만 확장합니다.|
    %~dp$PATH:\<element_variable_c\>|%\<element_variable_c\>에 대한 PATH 환경 변수 목록에 있는 Directory를 찾고 처음 찾은 것의 Drive 문자와 경로로 확장합니다.|
    %~ftza\<element_variable_c\>|%\<element_variable_c\>을 출력줄과 같은 DIR로 확장합니다.|

~~~batch
FOR /F "delims=" %%L IN ( "color/red.txt" ) DO (
    ECHO "LINE : "%%L
)
~~~

###### 3. until  
&nbsp;&nbsp;주어진 조건이 true가 될때까지 반복합니다. 즉, 주어진 조건이 false인 경우에만 반복합니다.    
<br>
&nbsp;**3.1. Shell script** 
- **until \<condition\> do ~ done**  

~~~sh
INDEX=0  
until [ ${INDEX} -gt 5 ]
do
    echo "while : "${INDEX}
    INDEX=$(($INDEX+1))
done
~~~  

&nbsp;**3.2. Batch file**  
&nbsp;&nbsp;Batch file에는 UNTIL문이 따로 없어서 WHILE문처럼 GOTO문을 사용하여 비슷하게 구현할 수 있습니다.
- **:WHILE ~ IF \<condition\> GOTO QUIT ~ GOTO WHILE ~ :QUIT**  

~~~batch
SETLOCAL ENABLEDELAYEDEXPANSION
SET INDEX=

:: WHILE문 시작 
:WHILE
ECHO "WHILE : "!INDEX!
SET /a INDEX+=1

::해당 조건을 만족하면 빠져나간다. 
IF !INDEX! GTR 5 (
    GOTO QUIT
) ELSE (
    GOTO WHILE
)

:: WHILE문 종료
:QUIT

ENDLOCAL
~~~  

###### 4. select
&nbsp;&nbsp;전달된 목록을 출력하고 그 목록 중 선택된 것에 대한 처리를 하는 반복문입니다.
Batch file에는 동일한 반복문이 없고, Shell에서도 Bash와 Korn에만 있습니다.  
<br>
&nbsp;&nbsp;**4.1. Shell script**  
- **select ${variable} in _${loop_array}_; do ~ done**  
다른 반복문과 다르게 내부에서 조건이 충족됩니다고 해서 반복문을 빠져나가지 않으므로 필요한 시점에 **break** 등을 사용해서  반복문을 빠져나가도록 처리해야 합니다.   
**${loop_array}**의 값은 자동으로 메뉴를 구성해주므로 사용자 입력(**${variable}**)을 받아 처리하는 데 사용할 수 있습니다.  

~~~sh
PS3="원하는 색상의 번호를 고르세요."
select var in ( "red" "green" "blue" ) 
do
    case ${var} in
        "red")
            echo "Selected! 1) red"
            break
            ;;
        "green")
            echo "Selected! 2) green"
            break
            ;;
        "blue")
            echo "Selected! 3) blue"
            break
            ;;
        *)
            echo "선택한 색상의 번호가 잘 못 되었습니다. 다시 입력해주세요"
            ;;
    esac
    REPLY=
done 
~~~

###### 5. 예약어  
&nbsp;&nbsp;반복문에서 사용되는 예약어입니다.  
<br>
&nbsp;&nbsp;**5.1. break**    
&nbsp;&nbsp;&nbsp;&nbsp;반복문을 종료합니다.

- Shell script  
~~~sh
file="color/green.txt"
while IFS='' read -r line || [ -n "$line" ]
do
    echo ${line}
    if [ "$line" == "jade green" ]
    then
        break;
    fi
done <"$file"
~~~

- Batch file   
~~~batch
FOR /F "delims=" %%L IN ( "color/green.txt" ) DO (
    ECHO "LINE : "%%L

    IF %%L == "jade green" (
        GOTO QUIT
    )
)
:QUIT
~~~  

&nbsp;&nbsp;**5.2. continue**  
&nbsp;&nbsp;&nbsp;&nbsp;continue 아래에 있는 부분을 건너뛰고 다음번 반복을 진행합니다.

- Shell script  

~~~sh
file="color/green.txt"
while IFS='' read -r line || [ -n "$line" ]
do
    if [ -z "$line" ]
    then
        continue;
    fi
    echo ${line}
done <"$file"
~~~
