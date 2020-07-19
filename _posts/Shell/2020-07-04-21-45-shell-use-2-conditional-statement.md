---
layout: post
title: "Shell의 사용 2 : 조건문"
crawlertitle: post_shell_use_2_conditional_statement
date: 2020-07-04 21:45:00 +0900
categories: shell 
summary: "Shell에서 조건문 사용.<br>제어문 중 조건문에 대한 내용입니다."
---
###### 1. 기본 조건문  
&nbsp;특정 조건에 대한 구문으로 Shell script와 Batch file 모두 **if ~ else**로 동일합니다.    
<br>
&nbsp;**1.1. Shell script** 

- **if [ _\<condition\>_ ]**    
조건이 하나만 있을 때 사용합니다.
~~~sh
if [ -z ${1} ]
then
    echo Empty Parameter 1
fi
~~~  

- **if [ _\<condition\>_ ] ~ else**  
조건의 true, false에 따라 구분될 때 사용합니다.  
~~~sh
START_INDEX=
if [ -z ${0} ]
then
    START_INDEX=0
else
    START_INDEX=${0}
fi
~~~  

- **if [ _\<condition_1\>_ ] ~ elif [ _\<condition_2\>_ ] ~ else**  
조건이 두 개 이상일 때, 두번째 조건부터 **elif(else if)**로 사용합니다. 
~~~sh
if [ -z ${0} ]
then
    START_INDEX=0
elif [ ${0} -ge 10  ]
    START_INDEX=10
else
    START_INDEX=${0}
fi
~~~

&nbsp;**1.2. Batch file**   
- **IF _\<condition\>_**  
조건이 하나만 있을 때 사용합니다.
~~~batch
IF [%1]=[] (
    ECHO Empty Parameter 1
)
~~~  

- **IF _\<condition\>_ ~ ELSE**   
조건의 true, false에 따라 구분될 때 사용합니다.  
~~~batch  
SETLOCAL ENABLEDELAYEDEXPANSION
SET START_INDEX=
IF [%1]=[] (
    SET START_INDEX=0
) ELSE (
    SET START_INDEX=%1
)
ENDLOCAL
~~~   

- **IF \<condition_1\> ~ ELSE IF \<condition_2\>~ ELSE**    
조건이 두 개 이상일 때, 두번째 조건부터 **ELSE IF**로 사용합니다.    
~~~batch 
SETLOCAL ENABLEDELAYEDEXPANSION
SET START_INDEX=
IF [%1]==[] (
    SET START_INDEX=0
) ELSE IF NOT %1 GEQ 10 (
    START_INDEX=10
) ELSE (
    SET START_INDEX=%1
)
ENDLOCAL
~~~  

###### 2. 경로(파일, 폴더)  
&nbsp;**2.1. Shell script**   
- 경로가 유효한지  
    - **-e \"${path}\"**  
    path에 해당하는 위치에 파일이나 폴더가 있으면 true, 그렇지 않으면 false  
    
    ~~~sh
    if [ -e "color/red.txt" ]
    then
        echo "Exists."
    else
        echo "Not exists."
    fi
    ~~~

- 파일인지, 폴더인지  
    - **-f \"${path}\"**
    path가 일반 파일이면 true, 폴더나 장치 파일인 경우 false  
    - **-d \"${path}\"**
    path가 폴더이면 true, 그렇지 않으면 false  

    ~~~sh
    if [ -f "color/green.txt" ]
    then
        echo "Regular file."
    fi
    if [ -d "color/achromatic" ]
    then
        echo "Directory"
    fi
    ~~~ 

- 파일의 크기가 0이 아닌지  
    - **-s \"${path}\"**  
    path에 해당하는 파일의 크기가 0이 아니면 true, 그렇지 않으면 false  

    ~~~sh
    if [ -s "color/blue.txt" ]
    then
        echo "blue.txt > 0"
    else
        echo "blue.txt Empty"
    fi
    ~~~

- 권한
    - **-r \"${path}\"**  
    path에 **읽기 권한**이 있는 경우 true, 그렇지 않으면 false  
    - **-w \"${path}\"**  
    path에 **쓰기 권한**이 있는 경우 true, 그렇지 않으면 false  
    - **-x \"${path}\"**  
    path에 **실행 권한**이 있는 경우 true, 그렇지 않으면 false  

    ~~~sh
    if [ -r "color/red.txt" ]
    then
        echo "Read permission"
    fi
    if [ -w "color/green.txt" ]
    then
        echo "Write permission"
    fi
    if [ -x "color/blue.txt" ]
    then
        echo "Execute permission"
    fi
    ~~~

- 수정일 비교
    - **\"${left_path}\" -nt \"${right_path}\"**  
    left_path의 파일이 right_path의 파일보다 최신입니다.  
    - **\"${left_path}\" -ot \"${right_path}\"**  
    left_path의 파일이 right_path의 파일보다 오래되었다.  

    ~~~sh
    if [ "color/red.txt" -nt "color/green.txt" ]
    then
        echo "red.txt Newer"
    elif [ "color/green.txt" -ot "color/blue.txt" ]
        echo "green.txt Older"
    fi
    ~~~

&nbsp;**2.2. Batch file**     
- 경로가 유효한지  
    - **EXIST \"%path%\"**  
    path에 해당하는 위치에 파일이나 폴더가 있으면 true, 그렇지 않으면 false  
    
    ~~~batch
    IF EXIST "color/red.txt" (
        ECHO "Exists."
    )
    ~~~

- 파일인지, 폴더인지  
    - **EXIST \"%path%/\"**
    경로 맨 끝에 **/**을 붙여 폴더가 있는 지 확인할 수 있습니다. path가 일반 폴더이면 true, 그렇지 않으면 경우 false  

    ~~~batch
    IF EXIST "color/achromatic/" (
        ECHO "Directory"
    ) ELSE (
        ECHO "File"
    )
    ~~~ 

###### 3. 정수 비교  
&nbsp;**3.1. Shell script** 
- 같음(Equal)  
    - **${left_variable} -eq ${right_variable}**  
    - **${left_variable} = ${right_variable}**  

    left_variable과 right_variable이 같음  
    ~~~sh  
    if [ ${0} -eq 0 ]
    then
        echo Zero
    elif [ ${0} == 1 ]
    then
        echo One
    fi
    ~~~  
     
- 같지 않음(Not Equal)  
    - **! ${left_variable} -eq ${right_variable}**  
    - **${left_variable} -ne ${right_variable}**  
    - **${left_variable} != ${right_variable}**  

    left_variable과 right_variable이 같지 않음  
    ~~~sh  
    if ! [ ${0} -eq 0 ]
    then
        echo "Not! Zero"
    elif [ ${0} -ne 1 ]
        echo "Not! One"
    elif [ ${0} != 2 ]
    then
        echo "Not! Two"
    fi
    ~~~  
    
- ~ 보다 큼(Great than)  
    - **${left_variable} -gt ${right_variable}**  
    - **((${left_variable} > ${right_variable}))**  

    left_variable이 right_variable보다 큼  
    ~~~sh  
    if [ ${0} -gt 10 ]
    then
        echo "Great than 10"
    elif (( ${0} > 0 ))
    then
        echo "Great than 0" 
    fi
    ~~~

- ~ 보다 작음(Less than)  
    - **${left_variable} -lt ${right_variable}**  
    - **((${left_variable} < ${right_variable}))**   

    left_variable이 right_variable보다 작음  
    ~~~sh  
    if [ ${0} -lt 0 ]
    then
        echo "Less than 0"
    elif (( ${0} < 10 ))
    then
        echo "Less than 10" 
    fi
    ~~~

- ~ 보다 크거나 같음(Great than or Equal)  
    - **${left_variable} -ge ${right_variable}**  
    - **((${left_variable} >= ${right_variable}))**  

    left_variable이 right_variable보다 크거나 같음  
    ~~~sh  
    if [ ${0} -ge 10 ]
    then
        echo "Great than Or Equal 10"
    elif (( ${0} >= 0 ))
    then
        echo "Great than Or Equal 0" 
    fi
    ~~~

- ~ 보다 작거나 같음(Less than or Equal)  
    - **${left_variable} -le ${right_variable}**  
    - **((${left_variable} <= ${right_variable}))**  

    left_variable이 right_variable보다 작거나 같음    
    ~~~sh  
    if [ ${0} -le 0 ]
    then
        echo "Less than Or Equal 0"
    elif (( ${0} <= 10 ))
    then
        echo "Less than Or Equal 10" 
    fi
    ~~~  

&nbsp;**3.2. Batch file** 

- 같음(Equal)  
    - **%left_variable% EQU %right_variable%**  
    - **%left_variable% == %right_variable%**  

    left_variable과 right_variable이 같음  
    ~~~batch  
    IF %1 EQU 0 (
        ECHO Zero
    ) ELSE IF %1 == 1 (
        ECHO One
    )
    ~~~  
     
- 같지 않음(Not Equal)  
    - **%left_variable% NEQ %right_variable%**  
    - **NOT %left_variable% == %right_variable%**  

    left_variable과 right_variable이 같지 않음  
    ~~~batch  
    IF %1 NEQ 0 (
        ECHO "Not! Zero"
    ) ELSE IF NOT %1 == 1 (
        ECHO "Not! One"
    )
    ~~~  
    
- ~ 보다 큼(Great than)  
    - **%left_variable% GTR %right_variable%**    

    left_variable이 right_variable보다 큼  
    ~~~batch  
    IF %1 GTR 10 (
        ECHO "Great than 10"
    )
    ~~~

- ~ 보다 작음(Less than)  
    - **%left_variable% LSS %right_variable%**  

    left_variable이 right_variable보다 작음  
    ~~~batch  
    IF %1 LSS 0 (
        ECHO "Less than 0"
    )
    ~~~

- ~ 보다 크거나 같음(Great than or Equal)  
    - **%left_variable% GEQ %right_variable%**  

    left_variable이 right_variable보다 크거나 같음  
    ~~~batch  
    IF %1 GEQ 10 (
        ECHO "Great than Or Equal 10"
    )
    ~~~

- ~ 보다 작거나 같음(Less than or Equal)  
    - **%left_variable% LEQ %right_variable%**  

    left_variable이 right_variable보다 작거나 같음    
    ~~~batch  
    IF %1 LEQ 0 (
        ECHO "Less than Or Equal 0"
    )
    ~~~  

###### 4. 문자열 비교  
&nbsp;**4.1. Shell script** 
- 같음(Equal)  
    - **\"${left_variable}\" = \"${right_variable}\"**
    - **\"${left_variable}\" == \"${right_variable}\"**  

    left_variable과 right_variable이 같음.
    = 앞 뒤로 공백에 주의해야 합니다.  
    ~~~sh     
    if [ "${0}" = "APPLE" ]
    then
        echo "It is apple."
    elif [ "${0}" == "GRAPE" ]
    then
        echo "It is grape."
    fi
    ~~~  
     
- 같지 않음(Not Equal)  
    - **\"${left_variable}\" != \"${right_variable}\"**  

    left_variable과 right_variable이 같지 않음  
    ~~~sh  
    if [ "${0}" != "APPLE" ]
    then
        echo "It isn't apple."
    fi
    ~~~  
    
- ASCII 알파벳 순서 비교시 ~보다 큼(Great than)   
    - **\"${left_variable}\" > \"${right_variable}\"**  

    left_variable이 right_variable보다 ASCII 알파벳 순서 비교시 큼  
    ~~~sh  
    if [ "${0}" > "APPLE" ]
    then
        echo "Great than Apple."
    fi
    ~~~

- ASCII 알파벳 순서 비교시 ~보다 작음(Less than)  
    - **\"${left_variable}\" < \"${right_variable}\"**   

    left_variable이 right_variable보다 ASCII 알파벳 순서 비교시 작음  
    ~~~sh  
    if [ "${0}" < "APPLE" ]
    then
        echo "Less than Apple."
    fi
    ~~~

- null  
    - **-z \"${variable}\"**  

    variable의 길이가 0 또는 null  
    ~~~sh  
    if [ -z "${0}" ]
    then
        echo "String is null." 
    else
        echo "String is not null."
    fi
    ~~~

- null이 아님(Not null)  
    - **-n \"${variable}\"**  

    variable이 null이 아님    
    ~~~sh  
    if [ -n "${0}" ]
    then
        echo "String is not null."
    else
        echo "String is null."
    fi
    ~~~  

&nbsp;**4.2. Batch file** 
- 같음(Equal)  
    - **\"%left_variable%\"==\"%right_variable%\"**  
    - **\"%left_variable%\" EQU \"%right_variable%\"**  
    - **[%left_variable%]==[%right_variable%]**  

    ~~~batch  
    IF %1=="APPLE" (
        ECHO "It is apple."
    ) ELSE IF %1 EQU "GRAPE" (
        ECHO "It is grape."
    ) 
    ~~~  
     
- 같지 않음(Not Equal)  
    - **NOT \"%left_variable%\"==\"%right_variable%\"**  
    - **\"%left_variable%\" NEQ \"%right_variable%\"**  

    left_variable과 right_variable이 같지 않음  
    ~~~batch  
    IF NOT %1=="APPLE"(
        ECHO "It isn't apple."
    ) ELSE IF %1 NEQ "GRAPE" (
        ECHO "It isn't grape.
    )
    ~~~

- null  
    - **\"%variable%\"==\"\"**
    - **[%variable%]==[]**  

    variable의 길이가 0 또는 null  
    ~~~batch  
    IF [%1]==[] (
        ECHO "String is null." 
    )
    IF "%2"=="" (
        ECHO "String is null."
    )
    ~~~

- 대소문자 구분없이 비교(Ignore case)  
    - **/I \"%left_variable%\" _\<compare_option\>_ \"%right_variable%\"**  

    left_variable과 right_variable를 대소문자 구분없이 비교    
    ~~~batch  
    IF /I %1 == "Apple" (
        ECHO "It's Apple."
    )
    ~~~  
