---
layout: post
title: "Shell의 사용 4 : 그 외 제어문"
crawlertitle: post_shell_use_4_other_control_-statement
date: 2020-07-11 22:13:00 +0900
categories: shell
summary: "Shell에서 조건문과 반복문을 제외한 제어문에 대한 내용입니다."
---
###### 1. 메뉴 선택  
&nbsp;비교할 조건이 여러 개일 때 사용하는 구문으로 switch와 비슷합니다.  
&nbsp;**1.1. Shell script**  
- **case _${variable}_ in _\<case_condition_1\>_) ~ ;; _\<case_condition_N\>_) ~ ;; ~ esac**    
단순히 정수를 비교하는 구문이 아니라 와일드카드 문자를 포함한 패턴도 사용할 수 있습니다.  
***)** 부분은 **switch ~ case**에서 **default**와 동일합니다.   
~~~sh
echo "경로를 입력하세요."
read  INPUT_PATH
case ${INPUT_PATH} in
    *.txt)
        echo ".txt 파일입니다."
        ;;
    *.png)
        echo ".png 파일입니다."
        ;;
    *.*)
        echo "파일입니다."
        ;;
    *) 
        echo "확장자가 없는 파일 또는 폴더입니다."
        ;;
esac
~~~  

&nbsp;**1.2. Batch file**  
- **CHOICE /C  _\<choices1...n\>_ _\<option\>_ /T _\<timeout\>_ /D _\<default_choice\>_ /M _\<display_text\>_**  
**ERRORLEVEL** 환경 변수에 선택된 값 대신 선택된 값의 Index가 설정됩니다. 선택 항목 순서대로 1부터 시작하며, 선택에 오류가 있는 경우 **255**가 설정됩니다. 사용자가 중지한 경우 ERRORLEVEL의 값은 0입니다.    
    - /C  _\<choices1...n\>_ : 유효한 선택 항목을 지정합니다. 따로 지정하지 않는 경우 기본 값은 "YN"입니다.  
    - /N : Prompt 끝에서 선택 목록과 '?'를 출력하지 않도록 합니다. 필요한 경우 "option"에 추가합니다.
    - /CS : 선택 항목에 대해 대소문자를 구분하여 지정합니다. 기본적으로 대소문자를 구분하지 않는다. 필요한 경우 "option"에 추가합니다.
    - /T _\<timeout\>_ : 항목이 선택될 때까지 대기할 시간(초)를 지정합니다. 범위는 0 ~ 9999이고 지정한 시간이 지나면 **/D**의 값이 기본 값으로 선택되므로 **/D**도 지정해야 합니다.
    - /D _\<default_choice\>_ : 기본 값
    - /M _\<display_text\>_ : Prompt에 표시할 문자열

~~~batch
:: y, n, c 중에서 선택 가능하며 5초 내에 선택되지 않으면 기본 값으로 'n'이 선택
CHOICE /C ync /T 5 /D n /M "y : Yes, n : No, c : Continue"
IF ERRORLEVEL 1 (
    GOTO YES
) ELSE IF ERRORLEVEL 2 (
    GOTO NO
) ELSE IF ERRORLEVEL 3 (
    GOTO CONTINUE
) ELSE (
    GOTO QUIT
)

:YES
ECHO "예"
GOTO QUIT

:NO
ECHO "아니오."
GOTO QUIT

:CONTINUE
ECHO "계속해서..."

:QUIT
exit 0
~~~  
