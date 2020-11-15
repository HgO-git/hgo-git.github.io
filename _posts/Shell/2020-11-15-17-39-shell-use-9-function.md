---
layout: post
title: "Shell의 사용 9 : 함수"
crawlertitle: post_shell_use_9_function
date: 2020-11-15 17:39:25 +0900
categories: shell 
summary: "Shell에서의 함수 사용"
---
###### 1. 함수  
&nbsp;Shell에서도 다른 프로그래밍 언어처럼 함수를 사용할 수 있습니다.  
&nbsp;**1.1. Shell script**  
- **function \<function_name\> { ~ }** 
- **\<function_name\>() { ~ }**  
function이 명시되어 있거나 함수 이름 뒤에 ``()``가 있는 경우 함수로 취급됩니다. 함수 이름 \<fuction_name\>을 ``{ }``에 지정하고 함수로 사용할 코드를 선언하면 됩니다. Shell script와 동일하게 function에서도 파라미터를 전달하여 사용할 수 있습니다.
- **function \<function_name\> { ~ return \<return_value\> }**  
- **\<function_name\>() { ~ return \<return_value\> }**  
필요에 따라 함수에서 값을 반환할 수 있습니다.  

~~~sh
function printFruitName
{
    if [ ${1} -eq 1 ]
    then
        echo "Apple"
    elif [ ${1} -eq 2 ]
    then
        echo "Banana"
    elif [ ${1} -eq 3 ]
    then
        echo "Grape"
    else
        echo "None"
    fi
}

addDecimal()
{
    echo "addDecimal Parameters : $@" 
    return $((${1} + ${2}))
}

## 반환된 값을 fruit_id 에 설정
addDecimal 0 1
fruit_id=$?

## fruit_id에 해당하는 과일 이름 출력
printFruitName ${fruit_id}
~~~

&nbsp;**1.2. Batch file**  
- **:\<function_name\> ~ EXIT /B \<exit_code\>**
함수 이름 \<function_name\>으로 Label을 정의하고 EXIT를 사용하여 함수가 정상적으로 종료하도록 합니다. EXIT의 \<exit_code\> 값이 0이 아니면 Error Level로 처리할 수 있습니다.  
- **CALL :\<function_name\> _\<parameter_1\>_ _\<parameter_2\>_ ... _\<parameter_n\>_**  
CALL을 사용하여 정의한 함수를 필요에 따라 전달인자와 함께 호출할 수 있습니다.  

~~~batch
SETLOCAL ENABLEDELAYEDEXPANSION

:: %1 : 과일 이름을 출력할 FRUIT_ID
:PRINT_FRUIT_NAME
    IF %1 EQU 1 (
        ECHO "Apple"
    ) ELSE IF %1 EQU 2 (
        ECHO "Banana"
    ) ELSE IF %1 EQU 3 (
        ECHO "Grape"
    ) ELSE (
        ECHO "None"
    )
    EXIT /B 0
:: END PRINT_FRUIT_NAME

:: %1, %2 : ADD Left, Right
:: %3 : 반환시 사용할 변수
:ADD_DECIMAL
    SET RESULT=%1+%2
    SET "%~3=%RESULT%"
    EXIT /B 0
:: END ADD_DECIMAL

SET FRUIT_ID=

:: 반환된 값을 fruit_id 에 설정
CALL :ADD_DECIMAL 0 1 FRUIT_ID

:: FRUIT_ID에 해당하는 과일 이름 출력
CALL :PRINT_FRUIT_NAME %fruit_id%

ENDLOCAL
~~~
