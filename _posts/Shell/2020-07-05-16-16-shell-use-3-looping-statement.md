---
layout: post
title: "Shell의 사용 3 : 반복문"
crawlertitle: post_shell_use_3_looping_statement
date: 2020-07-05 16:16:00 +0900
categories: shell 
summary: "Shell에서 반복문. 제어문 중 반복문에 대한 대한 내용이다."
---
###### 1. While    
&nbsp;주어진 조건이 true인 경우 반복되는 구문이다.  
&nbsp;**1.1. Shell script**   
- **while [ _\<condition\>_ ] do ~ done**
- **while [ _\<condition\>_ ]; do ~ done**
한 줄로 처리하고 싶을 때는 while과 조건 바로 뒤에 **;**를 추가한다.

~~~sh
INDEX=0
while [ ${INDEX} -lt 5  ]
do
    echo "while : "${INDEX}
    INDEX=$(($INDEX+1))
done
~~~  

&nbsp;**1.2. Batch file**  
&nbsp;&nbsp;Batch file에는 WHILE문이 따로 없어서 GOTO문을 사용하여 비슷하게 구현할 수 있다.
- **:WHILE ~ IF _\<condition\>_ GOTO QUIT ~ GOTO WHILE ~ :QUIT**  

~~~batch
SETLOCAL ENABLEDELAYEDEXPANSION
SET INDEX=

:: WHILE문 시작 
:WHILE
ECHO "WHILE : "!INDEX!
SET /a INDEX+=1

::해당 조건을 만족하면 반복한다. 
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
&nbsp;주어진 조건이 true인 경우 또는 주어진 배열까지 반복되는 구문이다.  
&nbsp;**2.1. Shell script**   
- **for ${variable} in ${loop_array} do ~ done**  
- **for (( _\<initializer\>;_ _\<condition\>_; _\<counting_expression\>_ ))**  
초기화한 변수를 특정 조건까지 반복한다.  

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
- **FOR _\<option\>_ %%A IN ( _\<loop_array\>_ ) DO ~**  
주어진 배열만큼 반복한다. 이때, 배열의 값에 대한 변수는 **%%**를 사용하며 변수명은 다른 변수와 중복되지 않아야 한다.

~~~batch
FOR /F "delims=" %%L IN ( "color/red.txt" ) DO (
    ECHO "LINE : "%%L
)
~~~

###### 3. until  
&nbsp;&nbsp;주어진 조건이 true가 될때까지 반복한다. 즉, 주어진 조건이 false인 경우에만 반복한다.    
&nbsp;**3.1. Shell script** 
- **until _\<condition\>_ do ~ done**  

~~~sh
INDEX=0  
until [ ${INDEX} -gt 5 ]
do
    echo "while : "${INDEX}
    INDEX=$(($INDEX+1))
done
~~~  

&nbsp;**3.2. Batch file** 
&nbsp;&nbsp;Batch file에는 UNTIL문이 따로 없어서 WHILE문처럼 GOTO문을 사용하여 비슷하게 구현할 수 있다.
- **:WHILE ~ IF _\<condition\>_ GOTO QUIT ~ GOTO WHILE ~ :QUIT**  

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

###### 4. 예약어  
&nbsp;&nbsp;반복문에서 사용되는 예약어이다.  
<br>

&nbsp;&nbsp;**4.1. break**    
&nbsp;&nbsp;반복문을 종료한다.

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

<br>

&nbsp;&nbsp;**4.2. continue**  
&nbsp;&nbsp;continue 아래에 있는 부분을 건너뛰고 다음번 반복을 진행한다.

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

- Batch file  

~~~batch
~~~