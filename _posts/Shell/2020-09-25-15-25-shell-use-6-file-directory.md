---
layout: post
title: "Shell의 사용 6 : File과 Directory"
crawlertitle: post_shell_use_6_file_directory
date: 2020-09-25 15:25:00 +0900
categories: shell 
summary: "Shell에서 파일과 디렉토리관련 내용"
---
###### 1. 생성  
&nbsp;File, Directory를 생성합니다.   
&nbsp;&nbsp;**1.1. Shell script**  
- **echo _\<content\>_ > \"${file_path}\"**  
\"${file_path}\" 위치에 _\<content\>_ 를 내용으로 하는 파일을 생성합니다. 

- **mkdir _\<option\>_ \"${new_directory_path}\"**  
\"${new_directory_path}\" 위치에 Directory를 생성합니다.   
    - -p : 필요시 _\<option\>_ 부분에 명시할 수 있는 옵션입니다. \"${new_directory_path}\"에 존재하지 않는 Directory가 포함된 경우 중간에 존재하지 않는 Directory도 함께 생성합니다. 

~~~sh
## Directory 생성
mkdir -p "work/project_01"
cd work
mkdir "project_02"

## File 생성
cd project_01
echo "title : shell" > info.txt
~~~  

&nbsp;&nbsp;**1.2. Batch file**  
- **ECHO _\<content\>_ > \"%file_path%\"**   
\"%file_path%\" 위치에 _\<content\>_ 를 내용으로 하는 파일을 생성합니다. 

- **MD \"%new_directory_path%\"**
- **MKDIR \"%new_directory_path%\"**   
**MD**와 **MKDIR**의 사용은 동일합니다.  
다른 옵션없이 \"%new_directory_path%\"에 존재하지 않는 Directory가 포함된 경우 함께 생성합니다.  
다수의 Directory를 생성한다면 **;(Colon, 콜론)**이나 **공백**으로 \"%new_directory_path%\"을 나열하면 됩니다.
	
~~~batch
:: Directory 생성
MKDIR "work/project_01"
CD work
MD "project_02"; "project_03" "project_04"

:: File 생성
CD project_01
ECHO "title : BATCH" > info.txt
~~~  

###### 2. 삭제  
&nbsp;File, Directory를 삭제합니다.  
&nbsp;&nbsp;**2.1. Shell script**  
- **rm _\<options\>_ \"${path}\"**  
\"${path}\"에 위치하는 File 또는 Directory를 삭제합니다. 필요시 아래와 같은 _\<option\>_ 을 사용할 수 있습니다.    
    - -r : Diretory 삭제시 하위의 내용을 먼저 삭제합니다. Recursive.  
    - -i : 삭제시 삭제할 것인지에 대해 묻습니다.  
    - -f : 삭제에 관련된 메세지를 보여주지 않고 강제로 삭제합니다. Force  
    - -v : 삭제되는 내용을 모두 출력합니다.  
- **rmdir _\<option\>_ \"${directory_path}\"**  
\"${directory_path}\"에 위치하는 Directory를 삭제합니다. **rm**과 다르게 Directory만 삭제합니다. 필요시 아래와 같은 _\<option\>_ 을 사용할 수 있습니다.  
    - -p : 삭제하려는 Directory의 상위 Directory가 비어있는 경우 함께 삭제합니다.  
    - -r : Diretory 삭제시 하위의 내용을 먼저 삭제합니다. Recursive.  
	
~~~sh
## 하위 파일과 함께 메세지 출력 없이 project_01 폴더 삭제
rm -rf "project_01"

## project_02 폴더 삭제로 비어 있을 work 폴더도 함께 삭제
rmdir -rp "project_02"
~~~  

&nbsp;&nbsp;**2.2. Batch file**  
- **DEL _\<option\>_ _\<attributes\>_ \"%paths%\"**  
- **ERASE _\<option\>_ \"%paths%\"**  
\"%paths%\"에 지정한 하나 이상의 File들을 삭제합니다. 와일드 카드를 사용하여 삭제할 수도 있습니다. 필요시 아래와 같은 _\<option\>_ 을 사용할 수 있습니다.  
    - /P : 각 파일을 삭제하기 전에 삭제를 확인하는 메시지를 표시합니다.
    - /F : 읽기 전용 파일을 강제 삭제합니다.
    - /S : 지정된 파일을 모든 하위 디렉터리에서 삭제합니다. 만약, 확장 기능을 사용한다면 찾지 못하는 파일이 아닌 지워진 파일을 보여줍니다.
    - /Q : 자동 모드에서는 글로벌 와일드카드에서 삭제할 것인지 묻지 않습니다.
    - /A : 특성을 기준으로 삭제할 파일을 지정합니다.
아래는 필요시 명시할 수 있는 _\<attributes\>_ 입니다.
    - /R : 읽기 전용 파일
    - /S : 시스템 파일
    - /H : 숨김 파일
    - /A : 보관할 파일
    - /I : 콘텐츠가 인덱싱되지 않은 파일
    - /L : 재분석 지점
    - /O : 오프라인 파일
    - \- : 부정을 뜻하는 접두사
- **RMDIR _\<option\>_ \"%directory_path%\"**  
- **RD _\<option\>_ \"%directory_path%\"**  
\"%directory_path%\"에 위치한 Directory를 삭제합니다. 필요시 아래와 같은 _\<option\>_ 을 사용할 수 있습니다.
    - /S : 지정된 Directory 자체와 그 안의 모든 File 및 Directory를 삭제합니다. Directory Tree를 삭제하는 데 사용합니다.
    - /Q : 조용한 모드로 Directory Tree를 삭제하는 데 문제가 없으면 다시 묻지 않습니다.

~~~batch
DEL /S /F "work_info.txt"
RMDIR /S /Q "work"
~~~  

###### 3. 복사  
&nbsp;File, Directory를 복사합니다.  
&nbsp;&nbsp;**3.1. Shell script**  
- **cp _\<option\>_ \"${source_path}\" \"${destination_path}\"**  
\"${source_path}\"에서 \"${destination_path}\"로 복사합니다.
- **cp _\<option\>_ \"${source_file_paths}\" \"${destination_directory_path}\"**    
\"${source_file_paths}\"에 명시된 파일들을 \"${destination_directory_path}\" 위치의 폴더로 복사합니다. 여러 개의 파일을 한번에 복사할 수 있습니다. 필요시 아래와 같은 _\<option\>_ 을 사용할 수 있습니다.
    - -a 또는 \--archive : 아카이브 파일. 원본 파일의 속성 및 링크 정보를 그대로 유지하면서 복사합니다. '-dR \--preserve=all'와 동일합니다.
    - \--attributes-only : 파일 정보는 복사하지 않고 속성만 복사합니다.
    - \--backup=_\<control\>_ : 복사될 위치에 이미 파일이 존재하는 경우 Backup 파일을 생성합니다. 인수를 사용하지 않을 수도 있습니다.
    - -b : '\--backup'과 비슷하지만 인수를 허용하지 않습니다.
    - \--copy-contents : 재귀시 특수 파일들의 내용을 복사합니다.
    - -d : '\--no-dereference_'와 '\--preserve=links'와 같습니다. 즉, 복사될 위치에 Symbolic Link, Hard Link, Junction과 같은 Link관련 정보가 있다면 유지하여 복사합니다.
    - -f 또는 \--force : 복사될 위치에 존재하는 파일을 열 수 없다면 삭제 후 복사합니다. 대신 '-n' 옵션 사용시 무시됩니다.
    - -i 또는 \--interactive : 덮어쓰기 전 Prompt에 확인 메세지를 출력합니다. 먼저 사용한 '-n' 옵션을 Override합니다.
    - -H : 원본의 command-line Symbolic Link를 따릅니다.
    - -l 또는 \--link : 복사하는 대신 Hard Link로 대채합니다.
    - -L 또는 \--dereference : 항상 원본의 Symbolic Link를 따릅니다.
    - -n 또는 \--no-clobber : 복사될 위치에 파일이 존재하면 덮어쓰지 않습니다. 먼저 사용한 '-i' 옵션을 Override합니다.
    - -P 또는 \--no-dereference : 절대 원본의 Symbolic Link를 따르지 않습니다.
    - -p : '\--preserve=mode,ownership,timestamps' 와 동일합니다.
    - \--preserve=_\<attributes\>_ : 지정한 속성을 보존합니다. 이때, 속성을 여러 개 입력이 가능하며 따로 입력하지 않아도 됩니다.  
        -- 기본 값 : mode, ownership, timestamps   
        -- 사용 가능한 속성 : context, links, xattr, all  
    - \--no-preserve=_\<attributes\>_ : 지정한 속성을 보존하지 않습니다. 이때, 속성을 여러 개 입력 가능합니다. 
    - \--parents : Directory 하위의 전체 원본 파일 이름을 사용합니다.
    - -R 또는 \--recursive : 재귀적으로 복사합니다. 폴더와 하위에 있는 파일과 폴더들을 모두 복사합니다.
    - \--reflink=_\<when\>_ : Clone/Copy-On-Write 복사본을 제어합니다.
    - \--remove-destination : 복사될 위치에 존재하는 파일을 열기 전에 삭제합니다. '\--force' 와 대비됩니다.
    - \--sparse=_\<when\>_ : 부족한 파일의 생성을 제어합니다.
    - \--strip-trailing-slashes : 명시된 원본 인수들에서 후행 슬래시( / )를 제거합니다.
    - -s 또는 \--symbolic-link : 복사하는 대신 Symbolic Link를 생성합니다.
    - -S 또는 \--suffix=_\<suffix\>_ : 일반적인 Backup의 접미사를 Override합니다.
    - -t 또는 \--target-directory=_\<directory\>_ : 모든 원본 인수들을 _\<directory\>_ 로 복사합니다.
    - -T 또는 \--no-target-directory : 복사될 위치의 파일을 일반 파일로 취급합니다.
    - -u 또는 \--update : 복사하려는 원본 파일이 복사될 위치의 파일보다 최신이거나 복사될 위치에 파일이 없을 때만 복사합니다.
    - -v 또는 \--verbose : 복사 상태에 대해 출력합니다.
    - -x 또는 \--one-file-system : 이 파일 시스템을 유지합니다.
    - -Z : 복사될 위치의 파일의 SELinux security context를 기본값으로 설정합니다.
    - \--context=_\<CTX\>_ : '-Z' 와 같거나 CTX가 지정된 경우 SELinux나 SMACK security context를 CTX로 설정합니다.

~~~sh
SOURCE_DIRECTORY_PATH="`pwd`/work/project_01"
DESTINATION_DIRECTORY_PATH="`pwd`/work/project_02"
cp -r -f ${DIRECTORY_PATH} ${DESTINATION_DIRECTORY_PATH}

cp -b fruits.txt animals.txt ${DESTINATION_DIRECTORY_PATH}
~~~  

&nbsp;&nbsp;**3.2. Batch file**  
- **COPY _\<option\>_ _\<file_encoding\>_ \"%source_paths%\" _\<file_encoding\>_ \"%destination_path%\" _\<file_encoding\>_**   
_\<file_encoding\>_ 은 따로 설정하지 않으면 기본적으로 ASCII 입니다. 아래는 사용할 수 있는 Encoding 입니다.
    - /A : ASCII 파일
    - /B : Binary 파일
\"%source_paths%\"에 명시된 파일 또는 폴더들을 \"%destination_path%\"로 복사합니다. 여러 개를 한번에 복사할 수 있습니다.   
필요시 아래와 같은 _\<option\>_ 을 사용할 수 있습니다.
    - /D : 대상 파일이 암호화 없이 만들어지도록 허용합니다.
    - /V : 새 파일이 정상적으로 쓰여지는지 확인합니다.
    - /N : 이름이 8자보다 길거나 확장자가 3자보다 긴 경우 짧은 파일 이름이 있으면 그 이름을 사용합니다.
    - /Y : \"%destination_path%\"에 이미 파일이나 폴더가 존재하면 확인 메세지 없이 덮어씁니다. COPYCMD 환경 변수에 이미 지정되어 있습니다.
    - /-Y : \"%destination_path%\"에 이미 파일이나 폴더가 존재하면 복사 전에 덮어쓸 것인지 확인 메세지를 출력합니다.
    - /Z : '다시 시작 가능 모드'로 파일을 복사합니다. 복사 도중에 중단되면 가능하면 다시 복사를 시작합니다. 약한 네트워크에서 사용됩니다.
         되면 가능하면 다시 시작됩니다. (저속 네트워크에서 사용)  '다시 시작 가능 모드'에서 네트워크에 연결된 파일을 복사합니다.
    - /L : \"%source_paths%\"에 위치한 파일이나 폴더가 기호화된 링크(Symbolic Link, Hard Link, Junction)인 경우 복사하는 대신 기호화된 링크를 생성합니다. 기호화된 링크가 가리키는 실제 원본을 복사합니다.  

- **XCOPY _\<option\>_ \"%source_paths%\" \"%destination_path%\"**   
\"%source_paths%\"에서 \"%destination_path%\"로 복사합니다. **COPY**의 확장 형태로 아래와 같은 다양한 옵션을 사용할 수 있습니다.
    - Source 옵션  
        -- /A : 원본의 아카이브 속성을 그대로 복사합니다. 기본값은 Y이므로 사용됩니다.  
        -- /H : 숨김 파일과 시스템 파일도 복사합니다.  
        -- /D:_\<date_format\>_ : _\<date_format\>_ 에 지정된 날짜 이후에 변경된 파일을 복사합니다. 따로 _\<date_format\>_ 을 설정하지 않으면 \"%destination_path%\"에 있는 파일 혹은 폴더의 날짜보다 최신의 날짜인 원본들만 복사합니다.  
        -- /E : 비어있는 폴더를 포함하여 재귀적으로 복사합니다. 비어 있는 경우를 포함하여 폴더와 하위에 있는 파일과 폴더들을 모두 복사합니다. '/T'를 수정하는 데 사용할 수 있습니다.  
        -- /EXCLUDE:_\<exclude_list_file_paths\>_ : 전체 경로나 일부 경로의 이름의 리스트로 구성된 파일을 설정하여 복사시 \"%source_paths%\"와 일부 일치하면 제외됩니다. _\<exclude_list_file_paths\>_ 에 여러 개의 파일을 명시할 수 있습니다. 예를 들어, exclude list에 .txt와 /work/ 가 있다면 work 폴더 내의 모든 파일을 제외하고 확장자가 .txt인 모든 파일도 제외합니다.  
        -- /M : 아카이브 속성이 설정된 원본을 복사하고 아카이브 속성을 해제합니다. 일반 백업을 만들때 사용됩니다. 기본값은 Y이므로 사용됩니다.  
        -- /S : 비어있는 폴더를 제외하고 재귀적으로 복사합니다. 비어있는 폴더를 제외하고 폴더와 하위에 있는 파일과 폴더들을 모두 복사합니다.  
        -- /U : \"%destination_path%\"에 이미 존재하는 경우만 복사합니다. Update  
    - Destination 옵션  
        -- /I : \"%destination_path%\"가 존재하지 않거나 두 개 이상의 파일을 복사하면 폴더로 가정합니다.  
        -- /K : 속성을 복사합니다. 그렇지 않으면 읽기 전용 속성을 다시 설정합니다.  
        -- /N : 가능하면 \"%destination_path%\"에 파일을 생성할 때 이름 8자, 확장자 3자까지인 짧은 이름을 사용합니다. 서로 다른 Format을 가진 Disk간에 복사할 때 필요할 수 있습니다. NTFS와 VFAT 같이 포맷이 다른 Disk간의 복사나 Data를 ISO9660 CDROM으로 보관하는 경우를 예로 들 수 있습니다.  
        -- /O : 파일의 소유권과 ACL 정보를 복사합니다.  
        -- /R : 읽기 전용 파일을 덮어씁니다.  
        -- /T : 파일은 복사하지 않고 폴더 구조만 생성합니다. 이때, 빈 폴더와 하위 폴더를 포함하지 않습니다. '/E'와 함께 사용하면 빈 폴더와 하위 폴더를 포함합니다.  
        -- /X : 파일의 감사(audit) 설정을 복사합니다. /O 의미.  
    - 공통 옵션  
        -- /B : 원본이 기호화된 링크인 경우 복사 대신 기호화된 링크를 생성합니다.  
        -- /C : 오류가 발생해도 복사를 계속합니다.  
        -- /F : 복사하는 동안 \"%source_paths%\"와 \"%destination_path%\"의 전체 경로를 출력합니다.  
        -- /G :  암호화 기능을 지원하지 않는 \"%destination_path%\"에 암호화된 \"%source_paths%\"를 복사할 수 있습니다.  
        -- /J : 버퍼를 사용하지 않는 I/O를 사용하여 복사합니다. 매우 큰 파일에 권장합니다.  
        -- /L : 복사할 파일들의 목록만 표시합니다.  
        -- /P : \"%destination_path%\" 각 파일을 생성하기 전 Prompt에 확인 메세지를 출력합니다.  
        -- /Q : 복사하는 동안 파일 이름을 출력하지 않습니다.  
        -- /V : \"%destination_path%\"에 생성되는 새 파일의 확인합니다. 읽을 수 있는 지, 크기, 아니면 기록될 때 확인합니다.  
        -- /W : 복사를 시작하기 전에 Prompt에 키 입력을 하라는 메세지를 출력합니다. 아무 키나 입력이 되어야 복사를 시작합니다.  
        -- /Y : \"%destination_path%\"에 이미 파일이나 폴더가 존재하면 확인 메세지 없이 덮어씁니다. COPYCMD 환경 변수에 이미 지정되어 있습니다.  
        -- /-Y : \"%destination_path%\"에 이미 파일이나 폴더가 존재하면 복사 전에 덮어쓸 것인지 확인 메세지를 출력합니다.  
        -- /Z : '다시 시작 가능 모드'로 파일을 복사합니다. 복사 도중에 중단되면 가능하면 다시 복사를 시작합니다. 약한 네트워크에서 사용됩니다.  

~~~batch
SET SOURCE_DIRECTORY_PATH="%CD%/work/project_01"
SET DESTINATION_DIRECTORY_PATH="%CD%/work/project_02"
XCOPY /S /Y %SOURCE_DIRECTORY_PATH% %DESTINATION_DIRECTORY_PATH%

COPY /B fruits.txt animals.txt %DESTINATION_DIRECTORY_PATH%
~~~  

###### 4. 존재 확인  
&nbsp;File, Directory이 실제로 있는 지 확인 합니다.  
&nbsp;&nbsp;**4.1. Shell script**  
- **-e \"${path}\"**
\"${path}\"에 해당하는 위치에 파일이나 폴더가 있으면 true, 그렇지 않으면 false  
	
~~~sh
if [ -e "color/red.txt" ]
then
    echo "Exists."
else
    echo "Not exists."
fi
~~~  

&nbsp;&nbsp;**4.2. Batch file**  
- **EXIST \"%path%\"**   
\"%path%\"에 해당하는 위치에 파일이나 폴더가 있으면 true, 그렇지 않으면 false  
	
~~~batch
IF EXIST "color/red.txt" (
    ECHO "Exists."
)
~~~  

###### 5. 이동  
&nbsp;지정 경로로 이동합니다.   
&nbsp;&nbsp;**5.1. Shell script**    
- **cd /"${path}\"**  
\"${path}\"의 경로로 이동합니다. 경로 대신 아래와 같은 문자를 사용하면 지정된 위치로 이동합니다.  
    - / : Root Directory로 이동합니다.
    - \- : 직전의 Directory로 이동합니다.
    - 빈 값 또는 ~ : 사용자의 Home Directory로 이동합니다.
    - .. : 상위 폴더로 이동합니다.

~~~sh
## Root Directory로 이동
cd /

## Work/project_01 경로로 이동
cd "work/project_01"

echo "project 01" >> info.txt

## 상위 폴더인 Work로 이동
cd ..
~~~  

&nbsp;&nbsp;**5.2. Batch file**  
- **CD _\</D\>_ \"%path%\"**   
- **CHDIR _\</D\>_  \"%path%\"**  
\"%path%\"의 경로로 이동합니다. **CD**와 **CHDIR**은 동일합니다.
    - /D : 현재 Drive까지 변경합니다.   
    - .. : 경로 대신 사용하면 상위 폴더로 이동합니다.

~~~batch
:: F 드라이브로 이동
CD /D F:

:: Work/project_01 경로로 이동
CD "work/project_01"

ECHO "project 01" >> info.txt

:: 상위 폴더인 Work로 이동
CD ..
~~~  

###### 6. 변경  
&nbsp;File, Directory의 이름을 변경합니다.  
&nbsp;&nbsp;**6.1. Shell script**  
- **mv _\<option\>_ \"${source_path}\" \"${destination_path}\"**  
\"${source_path}\"에 위치한 파일이나 폴더의 이름을 \"${destination_path}\"로 변경하거나 \"${source_path}\"에 있는 파일이나 폴더를 \"${destination_path}\"로 옮깁니다. 아래와 같은 옵션을 사용할 수 있습니다. 이때, -f, -i, -n 옵션은 서로 상반되는 기능이기 때문에 함께 사용하면 가장 마지막에 명시된 옵션만 적용됩니다.
    - \--backup=_\<control\>_ : 복사될 위치에 이미 파일이 존재하는 경우 Backup 파일을 생성합니다. 인수를 사용하지 않을 수도 있습니다.
    - -b : \--backup과 비슷하지만 인자를 사용하지 않습니다.
    - -f 또는 \--force : 덮어쓰기 전에 Prompt에 따로 확인 메세지를 출력하지 않습니다. _\<destination_path\>_ 에 이미 존재한다면 추가 메세지없이 덮어씁니다.
    - -i 또는 \--interactive : 덮어쓰기 전에 Prompt에 확인 메세지를 출력합니다.
    - -n 또는 \--no-clobber : 복사될 위치에 이미 파일이 존재하는 경우 덮어 쓰지 않습니다. 
    - \--strip-trailing-slashes : \"${source_path}\" 경로의 마지막 슬래시( / )를 제거합니다.
    - -S 또는 \--suffix=_\<suffix\>_ : 일반적인 Backup의 접미사를 Override합니다.
    - -t 또는 \--target-directory=_\<directory\>_ : move all SOURCE arguments into DIRECTORY
    - -T 또는 \--no-target-directory : 복사될 위치의 파일을 일반 파일로 취급합니다.
    - -u 또는 \--update : 복사하려는 원본 파일이 복사될 위치의 파일보다 최신이거나 복사될 위치에 파일이 없을 때만 복사합니다.
    - -v 또는 \--verbose : 복사 상태에 대해 출력합니다.
    - -Z 또는 \--context : 복사될 위치의 파일의 SELinux security context를 기본값으로 설정합니다.	

~~~sh
cd work

mv "project_01" "project_02"
~~~  

&nbsp;&nbsp;**6.2. Batch file**  
- **RENAME \"%source_file_path%\" \"%destination_file_name%\"**  
- **REN \"%source_file_path%\" \"%destination_file_name%\"**  
**RENAME**과 **REN**은 동일하게 \"%source_file_path%\"에 위치한 파일의 이름을 \"%destination_file_name%\"로 변경합니다. \"%destination_file_name%\"로 새 드라이브나 새 경로를 지정할 수 없습니다.  
- **MOVE _\<option\>_ \"%source_paths%\" \"%destination_path%\"**  
\"%source_paths%\"에서 \"%destination_path%\"로 파일이나 폴더를 옮기고 이름을 변경합니다. \"%source_paths%\"에 여러 개의 경로를 명시하면 한번에 여러 개의 파일을 옮길 수 있습니다. \"%destination_path%\"에는 드라이브를 포함한 전체 경로나 이름을 사용하면 됩니다. 아래는 사용 가능한 옵션입니다.  
    - /Y : \"%destination_path%\"에 이미 파일이나 폴더가 존재하면 확인 메세지 없이 덮어씁니다.  
    - /-Y : \"%destination_path%\"에 이미 파일이나 폴더가 존재하면 복사 전에 덮어쓸 것인지 확인 메세지를 출력합니다.

~~~batch
CD work

MOVE "project_01" "project_02"

CD project_02
REN info.txt info_prev.txt
~~~  

###### 7. File과 Directory 목록 확인  
&nbsp;현재 경로의 File과 Directory 목록을 확인합니다.  
&nbsp;&nbsp;**7.1. Shell script**  
- **ls _\<option\>_ _\<path\>_**  
기본적으로 현재 디렉토리 기준으로 파일에 대한 정보를 나열합니다. 옵션으로 -cftuvSUX나 \--sort가 지정되지 않은 경우 알파벳순으로 항목을 정렬합니다. 아래는 사용할 수 있는 옵션입니다.  
    - -a 또는 \--all : . 으로 시작하는 숨겨진 파일이나 폴더를 무시하지 않습니다.
    - -A 또는 \--almost-all : . 과 .. 을 묵시적으로 나열하지 않습니다.  
    - \--author : -l 옵션과 함께 사용되며 각 파일의 작성자를 출력합니다.
    - -b 또는 \--escape : 비출력 문자(Nongraphic Characters)에 대한 C-style Escapes를 출력합니다.
    - \--block-size=_\<size\>_ : 크기를 출력하기 전에 크기의 단위를 _\<size\>_ 로 조정합니다. 예를 들어, \--block-size=M 이면 MB(1,048,576 bytes)단위로 출력됩니다.
    - -B 또는 \--ignore-backups : ~ 로 끝나는 항목은 묵시적으로 나열하지 않습니다.
    - -c : -lt 옵션과 함께 사용하면 ctime( 파일 상태 정보의 마지막 수정 시간 )별로 정렬하여 표시합니다. -l 옵션과 함께 사용하면 ctime을 출력하고 이름별로 정렬하거나 ctime과 최신순으로 정렬합니다.
    - -C : 열(column)별로 항목을 나열합니다.
    - \--color=_\<when\>_ : 출력 색상을 설정합니다. _\<when\>_이 생략된 경우 기본적으로 always로 설정되어 있으며 그 외, auto 나 never 를 설정할 수 있습니다.
    - -d 또는 \--directory : 내용을 제외한 폴더들만 나열합니다.
    - -D 또는 \--dired : Emacs' dired mode용으로 디자인된 출력을 생성합니다.
    - -f : 정렬하지 않습니다. -aU 활성화, -ls \--color 비활성화됩니다.
    - -F 또는 \--classify : 항목에 indicator( */=>@\| )를 추가합니다.
    - \--file-type : 항목에 indicator( */=>@\| )를 추가하지만 '\*'을 추가하지 않습니다.
    - \--format=_\<word\>_ : format 설정을 합니다.(-x : across, -m : commas, -x : horizontal, -l : long, -1 : single-column, -l : verbose, -C : vertical)
    - \--full-time : -l \--time-style=full-iso 와 같습니다.
    - -g : -l와 같지만 소유자를 나열하지 않습니다.
    - \--group-directories-first : 파일보다 폴더 그룹을 먼저 나열합니다. \--sort 옵션을 사용하여 확장할 수 있지만 \--sort=none (-U) 을 사용하면 그룹화가 비활성화됩니다.
    - -G 또는 \--no-group : 나열된 목록이 긴 경우 그룹 이름을 출력하지 않습니다.
    - -h 또는 \--human-readable: -l 또는 -s와 함께 사용되며 사람이 읽을 수 있는 크기로 출력됩니다.(예 : 1K, 234M, 2G)
    - \--si : -l 또는 -s와 함께 사용되며 사람이 읽을 수 있는 크기로 출력되지만 1024가 아닌 1000의 제곱을 사용합니다.
    - -H 또는 \--dereference-command-line : 명령줄에 나열된 Symbolic Link를 따라갑니다.
    - \--dereference-command-line-symlink-to-dir : 폴더를 가리키는 각 명령줄의 Symbolic Link를 따라갑니다. 
    - \--hide=_\<pattern\>_ : Shell _\<pattern\>_ 과 일치하는 묵시적인 항목은 나열하지 않습니다. -a나 -A 옵션에 의해 재정의됩니다.
    - \--hyperlink=_\<when\>_ : 파일 이름에 하이퍼링크를 생성합니다.  _\<when\>_ 에는 'always', 'auto', 'never' 값 설정이 가능하며 따로 입력하지 않으면 기본값인 always로 설정됩니다.
    - \--indicator-style=_\<word\>_ : _\<word\>_ 스타일의 indicator가 진입명(Entry Name)에 추가됩니다. _\<word\>_ 의 기본값은 none이며, slash (-p), file-type (\--file-type), classify (-F) 를 사용할 수 있습니다.
    - -i 또는 \--inode : 각 파일의 인덱스 번호를 출력합니다.    
    - -I 또는 \--ignore=_\<pattern\>_ : Shell _\<pattern\>_ 과 일치하는 묵시적인 항목을 나열하지 않습니다.
    - -k 또는 \--kibibytes : Disk 사용시 1024 Byte 블록이 기본값입니다.
    - -l : 긴 목록 형식을 사용합니다. 
    - -L 또는 \--dereference : Symbolic Link에 대한 파일 정보를 표시할 때, Link 자체보다 Link가 참조하는 파일에 대한 정보를 표시합니다. 
    - -m : ,(쉼표)로 구분된 항목 목록으로 너비를  채웁니다.
    - -n 또는 \--numeric-uid-gid : -l과 비슷하지만 숫자로된 User와 Group ID를 나열합니다.
    - -N 또는 \--literal : 진입명(Entry Name)을 출력합니다.
    - -o : -l과 비슷하지만 Group 정보를 나열하지 않습니다.
    - -p 또는 \--indicator-style=slash : /(Slash)를 추가하여 폴더를 표시합니다.
    - -q 또는 \--hide-control-chars : 비출력 문자(Nongraphic Characters)는 ?로 대체하여 출력합니다.
    - \--show-control-chars : 비출력 문자(Nongraphic Characters)를 있는 그대로 출력합니다. 프로그램이 'ls'이 아니고 출력이 터미널이 아닌 경우 기본값입니다.
    - -Q 또는 \--quote-name : 진입명(Entry Name)을 큰따옴표(")로 감쌉니다.
    - \--quoting-style=_\<word\>_ : 진입명(Entry Name)에 _\<word\>_ 스타일의 인용을 사용합니다. _\<word\>_ 에는 literal, locale, shell, shell-always, shell-escape, shell-escape-always, c, escape 을 사용할 수 있습니다.
    - -r 또는 \--reverse : 역순으로 정렬합니다. 
    - -R 또는 \--recursive : 재귀적으로 하위 폴더까지 나열합니다.
    - -s 또는 \--size : 각 파일의 할당된 크기를 Block 단위로 출력합니다.
    - -S : 파일의 크기를 기준으로 정렬합니다. 가장 큰 크기의 파일이 가장 먼저 나옵니다. (파일 크기별 내림차순)
    - \--sort=_\<word\>_ : 이름 대신 설정한 _\<word\>_ 기준으로 정렬합니다. _\<word\>_ 에는 none (-U), size (-S), time (-t), version (-v), extension (-X) 를 사용할 수 있습니다.
    - \--time=_\<word\>_ : -l과 함께 사용되며, 기본적으로 수정된 시간 대신 _\<word\>_ 를 사용한 시간을 표시합니다. atime이나 access나 use (-u), ctime이나 status (-c), 또는 \--sort=_\<time\>_ (최신순으로 정렬)을 명시한 경우 시간을 정렬 키로 사용합니다.
    - \--time-style=_\<style\>_ : -l과 함께 사용되며, _\<style\>_ 을 사용하여 시간을 표시합니다. _\<style\>_ 에는 full-iso, long-iso, iso, locale, 또는 +_\<format\>_ 을 사용할 수 있습니다. _\<format\>_ 은 날짜와 같이 해석되며, _\<style\>_ 앞에 'posix-' 가 붙으면 POSIX locale 외부에서만 적용됩니다.
    - -t : 수정된 시간을 기준으로 최신순으로 정렬합니다.
    - -T 또는 \--tabsize=_\<clos\>_ : 8 대신 각 _\<cols\>_ 에 설정한 값으로 Tab stop을 가정합니다.
    - -u : -lt 와 함께 사용시 access time을 기준으로 정렬 및 표시합니다. -l 과 함께 사용시 이름 기준으로 정렬하고 access time을 표시합니다. 그 외 access time으로 가장 최신순으로 정렬합니다.
    - -U : 정렬하지 않고 폴더 순서대로 항목들을 나열합니다.
    - -v : 문자열 내의 버전과 같은 숫자를 기준으로 자연스럽게 정렬합니다.
    - -w 또는 \--width=_\<cols\>_ : _\<cols\>_ 값으로 출력 너비를 설정합니다. 0이면 제한이 없다는 의미입니다.
    - -x : 열 대신 행별로 항목을 나열합니다.
    - -X : 항목의 확장자에 따라 알파벳 기준으로 정렬합니다.
    - -Z 또는 \--context : 각 파일의 보안 Context를 출력합니다.
    - -1 : 한 줄당 하나의 파일을 나열합니다. -q나 -b와 함께 '\n'을 사용하면 안됩니다.  
    
~~~sh
cd "work/project_01"
ls -a 
~~~  
    
출력 결과 :  
~~~text
info.txt    project_01.project  library/temp.so
~~~  

&nbsp;&nbsp;**7.2. Batch file**  
- **DIR _\<path\>_ _\<options\>_**  
해당 경로의 파일과 폴더, 그리고 하위 폴더의 목록을 출력합니다. _\<path\>_ 에 출력할 Drive, Directory, File의 경로를 설정할 수 있습니다. 아래는 함께 사용할 수 있는 Option입니다.  
    - /A:_\<attributes\>_ : _\<attributes\>_ 에 지정한 속성을 가진 파일을 출력합니다. 아래는 _\<attributes\>_ 에 사용 가능한 파일 속성입니다.  
        -- D : 디렉터리  
        -- R : 읽기 전용 파일  
        -- H : 숨김 파일  
        -- A : 보관할 파일  
        -- S : 시스템 파일  
        -- I : 콘텐츠가 인덱싱되지 않은 파일  
        -- L : 재분석 지점  
        -- O : 오프라인 파일  
        -- - : _\<attributes\>_ 앞에 붙이면 해당 파일 속성을 제외합니다.  
    - /B : 최소 포맷을 사용합니다. 머리말 정보나 요약 없음
    - /C : 파일 크기에 천 단위 구분 기호를 표시합니다.  기본적으로 설정되므로 구분 기호를 표시하지 않으려면 /-C를 사용해야 합니다.
    - /D : 가로 목록과 동일하지만 파일 목록을 세로로 정렬하여 보여 줍니다.
    - /L : 소문자를 사용합니다.
    - /N : 파일 이름이 제일 오른쪽에 오도록 새로운 긴 목록 포맷을 사용합니다.
    - /O:_\<sortorder\>_ : _\<sortorder\>_ 기준으로 파일을 정렬하여 출력합니다. 아래는 _\<sortorder\>_ 에 설정할 수 있는 정렬 기준입니다.  
        -- N : 이름순(알파벳순)  
        -- S : 크기순(가장 작은 항목부터)  
        -- E : 확장명순(알파벳순)  
        -- D : 날짜/시간순(가장 오래된 항복부터)  
        -- G : 그룹 디렉터리 먼저  
        -- - : _\<sortorder\>_ 앞에 붙이면 역순으로 정렬합니다.  
    - /P : 출력된 내용으로 Console 창 화면이 가득차면 잠깐 멈춥니다.
    - /Q : 파일 소유자를 출력합니다.
    - /R : 파일의 대체 데이터 스트림을 표시합니다.
    - /S : 지정한 폴더와 그 하위 폴더까지 함께 출력합니다.(재귀적)  
	
~~~batch
CD "work/project_01"
DIR /S
~~~  

출력 결과 :   
~~~text
2020-06-05  오전 04:01      <DIR>       .
2020-08-15  오전 07:36                      info.txt
2020-08-15  오전 07:36                      project_01.project
2020-08-15  오전 07:36      <DIR>       library/temp.so
~~~  

###### 8. Link  
&nbsp;File, Directory의 Link를 설정, 해제합니다. Link는 보여지는 이름만 다를 뿐 원본과 동일한 File 또는 Directory를 공유하는 Hard Link와 다른 이름으로 보여질 뿐 실제로는 원본 File 또는 Directory에 접근하는 Symbolic Link 두 가지로 구분됩니다.  

&nbsp;&nbsp;**8.1. Shell script**  
- **ln _\<options\>_ "${source_path}" "${link_name}"**  
현재 경로에 ${source_path}를 ${link_name}으로 Link 생성합니다. 기본적으로 Hard link를 생성하므로 Symbolic Link를 생성하려면 \--symbolic 옵션과 함께 사용해야 합니다. 또한, 생성할 Link 위치에 Link든 실제 폴더나 파일이든 이미 대상이 있으면 안됩니다. Hard Link를 생성할 때 각 ${source_path}는 반드시 존재해야 합니다. Symbolic Link는 임의의 문자열을 포함할 수 있으며 이후 상대적 링크가 상위 폴더와의 관계에 의해 해석됩니다. 아래는 함께 사용할 수 있는 옵션입니다.
    - \--backup=_\<control\>_ : 복사될 위치에 이미 파일이 존재하는 경우 Backup 파일을 생성합니다. 인수를 사용하지 않을 수도 있습니다.
    - -b : '\--backup'과 비슷하지만 인수를 허용하지 않습니다.
    - -d 또는 -F 또는 \--directory : SuperUser가 Hard link 폴더에 접근 시도할 수 있도록 허용합니다. SuperUser라 하더라도 시스템 제한 사항으로 인해 실패할 수 있습니다.
    - -f 또는 \--force : Link가 생성될 위치에 이미 존재하는 파일이나 폴더를 삭제 후 Link를 생성합니다.
    - -i 또는 \--interactive : Link가 생성될 위치에 존재하는 파일이나 폴더를 삭제할 때 Prompt에 확인 메세지를 출력합니다.
    - -L 또는 \--logical : Symbolic Link인 ${source_path}의 역참조합니다.
    - -n 또는 \--no-dereference : Symbolic Link를 생성할 ${source_path}가 폴더인 경우 ${link_name}를 일반 파일로 취급합니다.
    - -P 또는 \--physical : Symbolic Link에 직접 Hard Link를 생성합니다.
    - -r 또는 \--relative : Link 위치에 상대적인 Symbolic Link를 생성한다.
    - -s 또는 \--symbolic : Hard Link 대신 Symbolic Link를 생성합니다.
    - -S 또는 \--suffix=_\<suffix\>_ : 일반적인 Backup 접미사를 재정의합니다. _\<suffix\>_ 를 설정한 경우 해당 값으로 재정의합니다.
    - -t 또는 \--target-directory=_\<directory\>_ : Link를 생성할 Directory를 지정합니다. _\<directory\>_ 에 Link 생성할 Directory를 명시하면 됩니다.
    - -T 또는 \--no-target-directory : ${link_name}을 항상 일반 파일로 취급합니다.  
- **rm _\<options\>_ "${link_path}"**  
${link_path}에 위치한 Link를 제거합니다. 아래는 함께 사용할 수 있는 옵션입니다.
    - -f 또는 \--force : 인자와 존재하지 않는 파일을 무시하며 Prompt에 출력하지 않습니다.
    - -i : Link를 제거하기 전 매번 Prompt에 확인 메세지를 출력합니다.
    - -I : 3개 이상의 파일 혹은 재귀적으로 삭제하기 전에 Prompt에 확인 메세지를 한번 출력합니다. 대부분의 실수를 방지하면서 -i 를 사용하는 것보다 덜 귀찮습니다.
    - \--interactive=_\<when\>_ : _\<when\>_ 설정에 따라 Prompt에 출력합니다. _\<when\>_ 에 설정 가능한 값은 never, once ( -l ), always ( -i ) 이며 기본값은 always 입니다.
    - \--one-file-system : 계층 구조를 재귀적으로 삭제할 때, 해당 명령줄 인자와 다른 파일 시스템의 폴더는 삭제하지 않고 건너뜁니다.
    - \--no-preserve-root : '/'를 특별하게 취급하지 않습니다.
    - \--preserve-root : '/'를 삭제하지 않습니다. 기본값입니다.
    - -r 또는 -R 또는 \--recursive : 폴더와 하위에 있는 파일과 폴더들을 재귀적으로 삭제합니다.
    - -d 또는 \--dir : 비어 있는 폴더들을 삭제합니다.

~~~sh
cd "shortcut"

ln -s "work/project_01" "p01"
rm -i "p00_test"
~~~  

&nbsp;&nbsp;**8.2. Batch file**  
- **MKLINK _\<options\>_ "%LINK_NAME%" "%SOURCE_PATH%"**  
현재 경로에 %SOURCE_PATH%를 %LINK_NAME%으로 Link 생성합니다. 필요에 따라 아래와 같은 옵션을 사용할 수 있습니다.  
    - /D : %SOURCE_PATH%에 대한 Link를 생성합니다. 기본값은 파일 바로 가기 링크입니다.
    - /H : 바로 가기 링크 대신 하드 링크를 만듭니다.
    - /J : 디렉터리 교차점을 만듭니다.  
- **RMDIR _\<options\>_ "%LINK_PATH%"**    
%LINK_PATH%에 위치한 Link를 제거합니다. 필요에 따라 아래와 같은 옵션을 사용할 수 있습니다.  
    - /S : 지정된 폴더와 그 하위의 폴더와 파일들을 모두 제거합니다. 폴더 구조를 일괄 제거하는 데 사용됩니다.
    - /Q : 조용한 모드로, /S로 제거하는데 문제가 없으면 다시 확인 메세지를 출력하지 않습니다.

~~~batch
CD "shortcut"

MKLINK /D "work/project_01" "p01"
RMDIR /S /Q "p00_test"
~~~  

###### 9. 비교  
&nbsp;두 개의 File이나 Directory를 비교합니다.  
&nbsp;&nbsp;**9.1. Shell script**  
- **cmp _\<options\>_  ${file_path_1} ${file_path_2}**  
${file_path_1}과 ${file_path_2}에 위치한 두 개의 파일을 Byte 단위로 비교합니다. 종료 값은 두 파일이 같으면 0, 다르면 1, 문제가 있으면 2입니다. 아래는 함께 사용할 수 있는 옵션입니다.
    - -b 또는 \--print-bytes : 다른 Bytes를 출력합니다.
    - -i 또는 \--ignore-initial=_\<skip\>_ : 비교할 두 파일의 시작부분에서 _\<skip\>_ Bytes만큼 건너뛰고 비교를 시작합니다. _\<skip\>_ 값은 지정하지 않으면 기본적으로 0며, Byte 대신 KB, K, MB, M, GB, G, T, P, E, Z, Y 와 같은 단위를 사용할 수 있으며 반드시 명시해야합니다.
    - -i 또는 \--ignore-initial=_\<skip_1\>_:_\<skip_2\>_ : 비교할 ${file_path_1} 시작부분에서는 _\<skip_1\>_ Bytes만큼, ${file_path_2} 시작부분에서는 _\<skip_2\>_ Bytes만큼 건너뛰고 비교를 시작합니다. 비교할 파일마다 건너 뛸 Bytes를 따로 지정할 수 있습니다. _\<skip_1\>_ 과 _\<skip_2\>_ 의 값은 지정하지 않으면 기본적으로 0입니다.
    - -l 또는 \--verbose : 다른 Byte 값과 번호를 출력합니다.
    - -n 또는 \--bytes=_\<limit\>_ : 최대로 _\<limit\>_ Bytes만큼만 비교합니다.
    - -s 또는 \--quiet 또는 \--silent : 모든 표준 출력을 하지 않습니다.  
- **comm _\<options\>_ ${file_path_1} ${file_path_2}**  
정렬된 ${file_path_1}과 ${file_path_2}에 위치한 두 개의 파일을 한 줄씩 비교합니다. 비교시 'LC_COLLATE'에 지정된 규칙을 따릅니다. 아래는 함께 사용할 수 있는 옵션입니다.
    - -1 : 첫 번째 열을 출력하지 않습니다. ${file_path_1}에 위치한 파일의 고유한 Line을 출력하지 않으므로 파일을 비교하여 ${file_path_2}에만 있는 고유한 내용과 공통된 내용만 출력합니다.
    - -2 : 두 번째 열을 출력하지 않습니다. ${file_path_2}에 위치한 파일의 고유한 Line을 출력하지 않으므로 파일을 비교하여 ${file_path_1}에만 있는 고유한 내용과 공통된 내용만 출력합니다.
    - -3 : 세 번째 열을 출력하지 않습니다. 비교한 두 파일의 공통된 내용을 출력하지 않고 각 파일들의 고유한 내용만 출력합니다. 기본값입니다.
    - \--check-order : 모든 입력줄이 페어링이 가능하더라도 입력이 바르게 정렬되었는 지 확입니다. 
    - \--nocheck-order : 입력이 바르게 정렬되어 있는 지 확인하지 않습니다.
    - \--output-delimiter=_\<str\>_ : 분리 문자열 _\<str\>_ 을 사용하여 열을 분리하여 출력합니다.
    - \--total : 요약하여 출력합니다.
    - -z 또는 \--zero-terminated : 줄 구분문자(Line delimiter)가 newline(\n, \r, \n\r)이 아니라 NUL 입니다.

- **diff _\<options\>_ ${path_1} ${path_2}**  
${path_1}과 ${path_2}에 위치한 파일이나 폴더를 한 줄씩 비교합니다. 종료 값은 두 파일이 같으면 0, 다르면 1, 문제가 있으면 2입니다. 아래는 함께 사용할 수 있는 옵션입니다.  
    - \--normal : 기본값이며, 일반 비교를 출력합니다.
    - -q 또는 \--brief : 파일들이 다를 때에만 알려줍니다.
    - -s 또는 \--report-identical-files : 두 파일이 같을 때에 알려줍니다.
    - -c 또는 -C _\<num\>_ 또는 \--context=_\<num\>_ : _\<num\>_ 의 기본 값은 3이며, 복사된 Context의 줄을 출력합니다.
    - -u 또는 -U _\<num\>_ 또는 \--unified=_\<num\>_ : _\<num\>_ 의 기본 값은 3이며, 통합된 Context의 줄을 출력합니다.
    - -e 또는 \--ed : ED Script를 출력합니다.
    - -n 또는 \--rcs : RCS 포맷이 다르면 출력합니다.
    - -y 또는 \--side-by-side : 두 열이 다르면 출력합니다.
    - -W 또는 \--width=_\<num\>_ : _\<num\>_ 의 기본 값은 130이며, _\<num\>_ 까지의 열만 출력합니다.
    - \--left-column : 공통 줄의 왼쪽 열만 출력합니다.
    - \--suppress-common-lines : 공통 줄을 출력하지 않습니다.
    - -p 또는 \--show-c-function : 각 변경 사항이 어떤 C 함수에 있는 지 표시합니다.
    - -F 또는 \--show-function-line=_\<re\>_ : _\<re\>_ 값과 일치하는 가장 최근의 줄만 표시합니다.
    - \--label _\<label\>_ : 파일의 이름과 타임스탬프 대신 _\<label\>_ 를 사용합니다. 반복 가능합니다.
    - -t 또는 \--expand-tabs : 출력에서 Tab을 공백문자로 확장합니다.
    - -T 또는 \--initial-tab : Tab을 미리 준비해서 Tab을 정렬시킵니다.
    - \--tabsize=_\<num\>_ : Tab이 _\<num\>_ 값이면 모든 열 출력을 멈춥니다. _\<num\>_ 의 기본 값은 8입니다.
    - \--suppress-blank-empty : 빈 출력 줄 전에 공백이나 Tab을 표시하지 않습니다.
    - -l 또는 \--paginate : 출력을 'pr'로 전달하여 페이지를 매깁니다.
    - -r 또는 \--recursive : 하위 폴더들에서도 재귀적으로 비교합니다.
    - \--no-dereference : Symbolic Link를 따라가지 않습니다.
    - -N 또는 \--new-file : 비교하려는 파일이 없는 경우 빈 파일로 취급합니다.
    - \--unidirectional-new-file : 비교하려는 첫 번째 파일이 없는 경우 빈 파일로 취급합니다.
    - \--ignore-file-name-case : 파일 이름 비교시 대소문자 구분하지 않습니다.
    - \--no-ignore-file-name-case : 파일 이름 비교시 대소문자를 구분합니다.
    - -x 또는 \--exclude=_\<pat\>_ : _\<pat\>_ 과 일치하는 파일을 제외합니다.
    - -X 또는 \--exclude-from=_\<file\>_ : _\<file\>_ 의 패턴과 일치하는 파일들은 제외합니다.
    - -S 또는 \--starting-file=_\<file\>_ : 폴더들을 비교시 _\<file\>_ 파일부터 비교를 시작합니다.
    - \--from-file=_\<file_1\>_ : _\<file_1\>_ 을 모든 피연산자(${path_2}에 명시한 경로의 파일들)와 비교합니다. _\<file_1\>_ 는 폴더가 될 수도 있습니다.
    - \--to-file=_\<file_2\>_ : _\<file_2\>_ 을 모든 피연산자(${path_1}에 명시한 경로의 파일들)와 비교합니다. _\<file_1\>_ 는 폴더가 될 수도 있습니다.
    - -i 또는 \--ignore-case : 파일 내용 비교시 대소문자를 구분하지 않습니다.
    - -E 또는 \--ignore-tab-expansion : 비교시 파일 내 Tab으로 인한 변경 사항을 무시합니다.
    - -Z 또는 \--ignore-trailing-space : 비교시 파일 내 줄 끝에 있는 White space를 무시합니다.
    - -b 또는 \--ignore-space-change : 비교시 파일 내 White space로 인한 변경 사항을 무시합니다.
    - -w 또는 \--ignore-all-space : 모든 White space를 무시합니다.
    - -B 또는 \--ignore-blank-lines : 비교시 파일 내 빈 칸으로만 이뤄진 줄에 의한 변경 사항을 무시합니다.
    - -I 또는 \--ignore-matching-lines=_\<re\>_ :  비교시 파일 내 _\<re\>_ 와 일치하는 모든 줄을 무시합니다.
    - -a 또는 \--text : 모든 파일을 Text로 취급합니다.
    - \--strip-trailing-cr : 입력시 끝에 있는 Carriage Return을 제거합니다.
    - -D 또는 \--ifdef=_\<name\>_  : '#ifdef _\<name\>_' 차이가 있는 병합된 파일을 출력합니다.
    - -d 또는 \--minimal : 가장 작은 차이점을 찾기위해 노력합니다.
    - \--horizon-lines=_\<num\>_ : 공통된 접두사와 접미사의 _\<num\>_ 줄을 유지합니다. 
    - \--speed-large-files : 큰 파일들과 많은 산재한 작은 변화들을 가정합니다.
    - \--color=_\<when\>_ : 출력에 색상을 입힙니다. _\<when\>_ 값은 'never', 'always', 'auto'이며 따로 명시하지 않았을 때 기본 값은 'auto'입니다.
    - \--palette=_\<palette\>_ : \--color 옵션이 활성화되어있을 때 사용할 색상을 설정합니다. _\<palette\>_ 는 ,(Colon)으로 구분된 Terminfo Capabilities 목록입니다.  
    - \--GTYPE-group-format=_\<gfmt\>_ : GTYPE 포맷의 입력 그룹을 _\<gfmt\>_ 포맷으로 처리합니다. GTYPE는 'old', 'new', 'unchanged', 'changed'입니다.    
    - \--line-format=_\<lfmt\>_ : 모든 입력 줄을 _\<lfmt\>_ 포맷으로 처리합니다. LTYPE는 'old', 'new', 'unchanged'입니다.
    - \--LTYPE-line-format=_\<lfmt\>_ : LTYPE 포맷의 입력 줄을 _\<lfmt\>_ 포맷으로 처리합니다.  

~~~sh
cd "work"

comm "project_01/info.txt" "project_02/info.txt"
diff "project_01" "project_02"
~~~  

&nbsp;&nbsp;**9.2. Batch file**  
- **COMP %file_path_1% %file_path_2% _\<options\>_**  
%file_path_1%과 %file_path_2%에 위치한 파일들을 비교합니다. %file_path_1%, %file_path_2%의 파일 경로에 와일드 카드를 사용하면 두 개 이상의 파일도 비교할 수 있습니다. 아래는 함께 사용할 수 있는 옵션입니다.  
    - /D : 다른 부분을 십진 형식으로 표시합니다.
    - /A : 다른 부분을 ASCII 문자로 표시합니다.
    - /L : 다른 부분에 대한 행 번호를 표시합니다.
    - /N=_\<number\>_ : 각 파일에서 지정한 번호인 _\<number\>_만큼의 행만을 비교합니다.
    - /C : ASCII 문자에 대해 대/소문자를 구별하지 않고 비교합니다.
    - /OFF _\<line\>_ : 오프라인 속성 세트 파일을 건너뛰지 않습니다.
    - /M : 다른 파일을 비교할지 여부를 묻는 메시지를 표시하지 않습니다.

- **FC _\<options\>_ %file_path_1% %file_path_2%**  
%file_path_1%과 %file_path_2%에 위치한 파일 또는 파일의 집합을 비교하고 다른 점을 화면에 출력합니다. 아래는 함께 사용할 수 있는 옵션입니다.  
    - /B : 이진 모드에서 비교합니다. 이진 모드로 비교하는 경우 다른 옵션을 사용할 수 없습니다.
    - /A : 연속적으로 차이가 있는 부분의 첫 번째 줄과 마지막 줄만 표시합니다.
    - /C : 대/소문자를 구별하지 않습니다.
    - /L : ASCII 파일로 취급하고 비교합니다.
    - /LBn : 연속적으로 차이가 있는 부분의 최대 줄의 수를 지정합니다.
    - /OFF _\<line\>_ : 오프라인 속성 세트 파일을 건너뛰지 않습니다.
    - /N : ASCII 문자로 비교 중 행 번호를 표시합니다.
    - /T : 탭을 공백으로 확장하지 않습니다.
    - /U : Unicode 파일로 취급하고 비교합니다.
    - /W : 비교 중 빈 공간(탭과 공백)을 압축합니다.
    - /nnnn : 같지 않은 줄 다음에 연속적으로 같아야 하는 줄의 수를 지정합니다.

~~~batch
CD "work"

COMP "project_01/info.txt" "project_02/info.txt"
FC "project_01" "project_02"
~~~  

###### 10. 정렬    
&nbsp;File 또는 표준 입력을 정렬하여 출력합니다.  
&nbsp;&nbsp;**10.1. Shell script**  
- **sort _\<options\>_ ${file_paths}**  
명시한 모든 ${file_paths}에 위치한 파일들의 연결을 정렬하여 표준 출력으로 씁니다. ${file_paths} 가 없거나 - 인 경우 표준 입력으로 읽습니다. 아래는 함께 사용할 수 있는 옵션입니다.
    - -b 또는 \--ignore-leading-blanks : 선행 공백을 무시합니다.
    - -d 또는 \--dictionary-order : 공백이나 알파벳과 숫자로 구성된 문자만 고려합니다.
    - -f 또는 \--ignore-case : 대소문자 구분하지 않습니다.
    - -g 또는 \--general-numeric-sort : 일반 숫자 값에 따라 비교하여 정렬합니다.
    - -i 또는 \--ignore-nonprinting : 인쇄 가능한 문자만 고려합니다.
    - -M 또는 \--month-sort : '월' 기준으로 비교합니다. (월을 모르는 경우) < 'JAN' < 'DEC' 순으로 정렬됩니다.
    - -h 또는 \--human-numeric-sort : 사람이 읽을 수 있는 숫자를 비교합니다. 예로 2K, 1G
    - -n 또는 \--numeric-sort : 문자로 된 숫자 값에 따라 비교하여 정렬합니다.
    - -R 또는 \--random-sort : 임의로 정렬하지만 동일한 키들을 그룹화합니다. shuf(1) 참조
    - \--random-source=_\<file\>_ : _\<file\>_ 에서 임의의 Bytes를 가져옵니다.
    - -r 또는 \--reverse : 비교 결과의 반대로 정렬합니다.
    - -V 또는 \--version-sort : 텍스트 내 자연적인 숫자(Version)에 따라 정렬합니다.
    - \--sort=_\<word\>_ : _\<word\>_ 에 따라 정렬합니다. _\<word\>_ 에는 일반 숫자(general-numeric) -g, 사람이 읽을 수 있는 숫자(human-numeric) -h, 월(month) -M, 숫자(numeric) -n, 랜덤(random) -R, 버전(version) -V 를 사용합니다.
    - \--batch-size=_\<nmerge\>_ : 임시 파일들을 최대로 사용하기 위해 입력한 최대  _\<nmerge\>_ 를 한번에 병합합니다.
    - -c 또는 \--check 또는 \--check=diagnose-first : 따로 정렬하지는 않고 정렬된 입력을 확인합니다.
    - -C 또는 \--check=quiet 또는 \--check=silent : -c 와 비슷하지만 잘못된 첫 번째 줄은 보고하지 않습니다.
    - \--compress-program=_\<prog\>_ : 지정한 압축 프로그램 _\<prog\>_ 를 사용하여 임시로 압축합니다. _\<prog\>_ -d로 사용하면 압축 해제를 합니다. 
    - \--debug : 정렬을 사용한 줄부분에 주석을 달고 stderr의 의심스러운 사용에 대하여 경고합니다.
    - \--files0-from=_\<f\>_ : 지정한 파일 _\<f\>_ 에서 NUL로 끝나는 이름으로 지정된 파일들을 입력을 읽습니다. 만약 _\<file\>_ 가 -이면 표준 입력에서 이름을 읽습니다. 
    - -k 또는 \--key=_\<keydef\>_ : 키를 통해 정렬합니다. _\<keydef\>_ 는 F[.C][OPTS][,F[.C][OPTS]] 로 위치와 형식을 제공합니다. 시작과 중지 위치에 대한 것이면 F는 Field 번호, C는 Field에서 문자의 위치입니다. F와 C가 모두 1이면 정치 위치는 기본적으로 줄 끝에 위치합니다. -t와 -b가 모두 유효하지 않으면 Field 내 문자는 앞의 WhiteSpace부터 시작하여 계산합니다. OPTS는 해당 Key에 대한 전역 순서 옵션을 재정의하는 하나 이상의 단일 문자 순서 옵션 [bdfgiMhnRrV]이다. 만약 Key가 없는 경우 전체 줄을 Key를 사용합니다. \--debug 옵션을 사용하여 잘못된 Key 사용을 진단할 수 있습니다.
    - -m 또는 \--merge : 이미 정렬된 파일들을 병합합니다. 따로 정렬하지 않습니다.
    - -o 또는 \--output=_\<file\>_ : 표준 출력 대신 _\<file\>_ 로 결과를 출력합니다.
    - -s 또는 \--stable : 마지막 비교를 비활성화하여 정렬을 안정화합니다.
    - -S 또는 \--buffer-size=_\<size\>_ : Main Memory Buffer에 사용할 _\<size\>_ 를 지정합니다. _\<size\>_ 는 곱셈, Memory, 등의 접미사를 사용할 수 있습니다. 접미사 K가 기본값이며 그 외 b, M, G, T, P, E, Z, Y 등을 사용할 수 있습니다.
    - -t 또는 \--field-separator=_\<sep\>_ : 공백을 _\<sep\>_ 을 사용하여 대체합니다.
    - -T 또는 \--temporary-directory=_\<dir\>_ : _\<dir\>_ 에 위치한 폴더를 임시로 사용합니다. $TMPDIR 이나 /tmp 는 사용할 수 없으며 옵션을 여러 개 사용하여 여러 개의 폴더를 지정할 수 있습니다. 
    - \--parallel=_\<n\>_ : 동시에 정렬을 진행할 수를 _\<n\>_ 로 설정합니다.
    - -u 또는 \--unique : -c 와 함께 사용하면 엄격하게 순서를 확인하고 -c와 함께 사용하지 않으면 동일한 실행 중 첫 번째만 출력합니다.
    - -z 또는 \--zero-terminated : 줄 구분자가 NewLine이 아니라 NUL 입니다.

~~~sh
cd "work"
sort -n 3 "project_01/info.txt" "project_02/info.txt"
~~~  

&nbsp;&nbsp;**10.2. Batch file**    
- **SORT _\<optoins\>_ %file_paths%**  
%file_paths% 위치에 있는 파일을 정렬합니다. 따로 지정한 %file_paths%가 없으면 표준 입력이 정렬되며 %file_paths%를 지정하면 같은 파일을 리디렉션하는 것보다 빠릅니다. 아래는 함께 사용할 수 있는 옵션입니다.
    - /+_\<n\>_ : 각 비교를 시작할 문자 번호 _\<n\>_ 을 지정합니다. _\<n\>_ 미만의 줄은 다른 줄보다 먼저 정렬됩니다. 기본적으로 비교는 각 줄의 첫 번째 문자에서 시작합니다. 예로, /+3 이면 각 비교가 각 줄의 3번쨰 문자부터 시작되어야 한다는 것을 나타냅니다.
    - /L _\<locale\>_ 또는 /LOCALE _\<locale\> : 시스템의 기본 Locale을 지정한 _\<locale\>_ 로 재정의합니다. "C" locale은 가장 빠른 조합 순서를 생성하며 현재 유일한 대안입니다. 해당 정렬은 항상 대소문자를 구분하지 않습니다.
    - /M _\<kilobytes\>_ 또는 /MEMORY _\<kilobytes\>_ : 정렬시 사용할 Main Memory의 양(Kilobytes)을 설정합니다. 메모리 크기는 항상 최소 160 KB로 제한됩니다. Main memory의 가용 크기에 상관하지 않고 지정한 메모리 크기만큼 정확한 양을 정렬에 사용합니다. 보통 메모리 크기를 설정하지 않으면 최상의 퍼포먼스를 얻을 수 있습니다. 기본적으로 기본 최대 메모리 크기에 맞을 때, 한 번의 한 번의 pass(임시 파일 없음)로 정렬이 되고 그렇지 않으면 두번의 pass(부분적을 정렬된 Data가 임시 파일에 저장)에서 모든 파일의 정렬과 병합에 사용되는 메모리 크기의 양이 같도록 정렬이 진행된다. 기본 최대 메모리 크기는 입출력 파일이 Main memory에서 사용 가능한 기본 최대 메모리 크기는 90%이고 입출력이 파일인 경우 Main memory의 90%, 그 외의 경우 45%까지 사용가능합니다.
    - /REC _\<char_count\> 또는 /RECORD_MAXIMUM _\<char_count\> : Record 내 최대 문자의 수를 _\<char_count\> 로 설정합니다. 기본 값은 4,096, 최대 값은 65,535 입니다.
    - /R 또는 /REVERSE : 정렬을 역순으로 변경합니다.
    - /T _\<temp_directory_path\>_ 또는 /TEMPORARY _\<temp_directory_path\>_ :  정렬 작업에 필요한 데이터가 Main memory를 초과하는 경우 해당 내용을 저장할 Directory 경로를 지정합니다. 기본 경로는 시스템 임시 디렉터리입니다.
    - /O _\<output_file_path\>_ 또는 /OUTPUT _\<output_file_path\>_ : 정렬된 데이터를 저장할 파일 경로를 지정합니다. 지정하지 않으면 표준 출력 파일에 기록됩니다.  출력 파일을 지정하면 같은 파일을 리디렉션하는 것보다 빠릅니다.
	
~~~batch
CD "work"
SORT /R "project_01/info.txt" "project_02/info.txt"
~~~  

###### 11. 화면당 출력  
&nbsp;File의 Perusal Filter로 File을 Prompt 한 화면에 한번씩 나눠서 출력합니다.  
&nbsp;&nbsp;**11.1. Shell script**   
- **more _\<options/>_ ${file_path}**  
Terminal에서 보기 위한 파일 열람 필터로 ${file_path} 위치의 파일을 출력합니다. 아래는 함께 사용할 수 있는 옵션입니다. 
    - -d : 경고 대신 도움 문구를 표시합니다.
    - -f : 화면의 줄보다는 논리적인 계산을 합니다.
    - -l : 양시을 Feed 한 후 일시 중지를 하지 않도록 제한합니다.
    - -c : Scroll, 텍스트 표시, 줄 끝 정리를 하지 않습니다.
    - -p : Scroll, 화면 지움, 텍스트 표시를 하지 않습니다.
    - -s : 여러 개의 빈 줄을 하나의 줄로 처리합니다.
    - -u : 밑줄을 제한합니다.
    - \- _\<number\>_ : 화면당 줄 수를 설정합니다.
    - +_\<number\>_ : 설정한 줄 번호부터 파일에서 화면으로 표시하기 시작합니다.
    - +/_\<string\>_ : 파일에서 설정한 _\<string\>_ 문자열과 일치하는 부분부터 화면으로 표시하기 시작합니다.  

~~~sh
cd "work/project_01"
more -c "info.txt"
~~~  

&nbsp;&nbsp;**11.2. Batch file**  
- **MORE _\<options\>_ < %file_path%**    
파일 %file_path% 내용을 한 번에 한 화면씩 출력합니다. 
- **MORE /E _\<options\>_ %file_paths%**   
파일 경로 목록 %file_paths%의 내용을 한 번에 한 화면씩 출력합니다. 파일 경로 목록 %file_paths%의 파일은 공백으로 구분합니다. /E 옵션으로 확장 기능을 사용할 수 있습니다.  
- **_\<command\>_ | MORE _\<options\>_ %file_path%**  
결과를 화면에 표시할 명령 _\<command\>_ 를 지정할 수 있습니다.    
아래는 함께 사용할 수 있는 옵션입니다.
    - /C : 페이지를 표시하기 전에 화면을 지웁니다.
    - /P : FF(form-feed) 문자를 확장합니다.
    - /S : 러 개의 빈 줄을 하나의 빈 줄로 바꿉니다.
    - /Tn : 탭을 n개(기본값은 8)의 공백으로 바꿉니다.
    - +_\<n\>_ : 첫 번째 파일을 _\<n\>_ 줄에서부터 출력합니다.
    - /E : 확장 기능을 사용할 수 있게 합니다.
    - P _\<n\>_ : (확장 기능) 다음 _\<n\>_ 줄을 표시합니다.
    - S _\<n\>_ : (확장 기능) 다음 _\<n\>_ 줄을 건너뜁니다.
    - F : (확장 기능) 다음 파일을 표시합니다.
    - Q : (확장 기능) 마칩니다.
    - = : (확장 기능) 줄 번호를 표시합니다.
    - ? : (확장 기능) 도움말을 표시합니다.
    - [공백] : (확장 기능) 다음 페이지를 표시합니다.
    - [Enter] : (확장 기능) 다음 줄을 표시합니다.

~~~batch
CD "work/project_01"
MORE /C < "info.txt"
~~~  

###### 12. 파일 안에서 문자열 찾기   
&nbsp;File에서 문자열을 찾습니다.  
&nbsp;&nbsp;**12.1. Shell script**   
- **grep _\<options\>_ ${pattern} ${file_paths}**  
명시된 ${file_paths} 각 파일에서 ${pattern}과 일치하는 문자열을 찾습니다. ${file_paths}가 '-'이면 표준 입력을 읽고 ${file_paths}가 없을 때 재귀적이면 '.' 재귀적이지 않으면 '-'로 읽습니다. ${file_paths}가 2개 이하이면 -h로 가정합니다. 줄을 선택하면 종료 상태는 0, 그렇지 않으면 1, 에러가 발생하였을 때 -q 옵션이 사용되지 않았으면 2입니다. 아래는 함께 사용할 수 있는 옵션입니다.
    - -E 또는 --extended-regexp : ${pattern}에 확장된 정규 표현식을 사용할 수 있습니다. 
    - -F 또는 --fixed-strings : ${pattern}은 Newline으로 구분된 문자열들로 구성됩니다.
    - -G 또는 --basic-regexp : ${pattern}은 기본 정규 표현식을 사용합니다. 기본 값입니다.
    - -P 또는 --perl-regexp : ${pattern}은 Perl 정규 표현식입니다.
    - -e 또는 --regexp=_\<pattern\>_ : 옵션에 설정한 _\<pattern\>_ 과 일치
    - -f 또는 --file=_\<file\>_ : _\<file\>_ 에서 ${pattern}으로 사용할 값을 가져옵니다.
    - -i 또는 --ignore-case : 대소문자를 무시합니다.
    - -w 또는 --word-regexp : ${pattern}이 전체 단어에 일치하도록 강제 설정합니다.
    - -x 또는 --line-regexp : ${pattern}이 전체 줄에 일치하도록 강제 설정합니다.
    - -z 또는 --null-data : Data 줄의 끝은 Newline이 아닌 0 byte로 끝납니다.
    - -s 또는 --no-messages : 오류 메세지를 표시하지 않습니다.
    - -v 또는 --invert-match : 일치하지 않은 줄을 선택합니다.
     (출력 제어) 
    - -m 또는 --max-count=_\<num\>_ : 선택한 _\<num\>_ 번째 줄 이후로 멈춥니다.
    - -b 또는 --byte-offset : 줄마다 Byte offset과 함께 출력합니다.
    - -n 또는 --line-number : 줄마다 줄 번호와 함께 출력합니다.
    - --line-buffered : 모든 줄에 출력 Flush
    - -H 또는 --with-filename : 줄마다 파일 이름과 함께 출력합니다.
    - -h 또는 --no-filename : 출력시 파일 이름 접두사를 표시하지 않습니다.
    - --label=_\<label\>_ : 표준 입력 파일 이름 접두사 대신 _\<label\>_ 을 사용합니다.
    - -o 또는 --only-matching : 줄에서 ${pattern}과 일치하는 부분만 표시합니다.
    - -q 또는 --quiet 또는 --silent : 일반 출력을 모두 표시하지 않습니다.
    - --binary-files=_\<type\>_ : Binary 파일들을 _\<type\>_ 으로 가정합니다. _\<type\>_ 은 'binary', 'text', 'without_match' 로 구성됩니다.
    - -a 또는 --text : --binary-files=text 와 동일합니다.
    - -I : --binary-files=without-match 와 동일합니다.
    - -d 또는 --directories=_\<action\>_ : 폴더들의 처리 방법으로 _\<action\>_ 은 'read', 'recurse', 'skip'을 사용 할 수 있습니다.
    - -D 또는 --devices=_\<action\>_ : FIFO, Socket, Device의 처리 방법으로  _\<action\>_ 은 'read', 'skip'을 사용 할 수 있습니다.
    - -r 또는 --recursive : --directories=recurse 와 동일합니다.
    - -R 또는 --dereference-recursive : -r 과 비슷하지만 모든 Symbolic Link를 따릅니다.
    - --include=_\<file_pattern\>_ : _\<file_pattern\>_ 와 일치하는 파일들만 검색합니다.
    - --exclude=_\<file_pattern\>_ : _\<file_pattern\>_ 와 일치하는 파일이나 폴더는 생략됩니다.
    - --exclude-from=_\<file\>_ : _\<file\>_ 에서 file pattern과 일치하는 파일은 생략됩니다.
    - --exclude-dir=_\<pattern\>_ : _\<pattern\>_ 과 일치하는 폴더는 생략됩니다.
    - -L 또는 --files-without-match : 선택하지 않은 줄은 ${file_paths} 이름만 출력합니다.
    - -l 또는 --files-with-matches : 선택한 줄은 ${file_paths} 이름만 출력합니다.
    - -c 또는 --count : 파일당 선택한 줄의 수만 출력합니다.
    - -T 또는 --initial-tab : 필요한 경우 탭 정렬을 합니다.
    - -Z 또는 --null : 파일 이름 뒤에 0 byte를 출력합니다.
    - -B 또는 --before-context=_\<num\>_ : 선행 Context의 _\<num\>_ 번째 줄을 출력합니다.
    - -A 또는 --after-context=_\<num\>_ : 후행 Context의 _\<num\>_ 번째 줄을 출력합니다.
    - -C 또는 --context=_\<num\>_ : 출력 Context의 _\<num\>_ 번째 줄을 출력합니다.
    - -NUM : --context=_\<num\>_ 과 동일합니다.
    - --color=_\<when\>_ 또는 --colour=_\<when\>_ : 일치하는 문자열에 Marker를 사용하여 강조 표시를 합니다. _\<when\>_ 에는 'always', 'never', 'auto' 를 사용할 수 있으며 빈 값으로 두면 기본 값인 'auto'입니다.
    - -U 또는 --binary : EOL(MSDOS/Windows)에서 CR 문자를 제거하지 않습니다.
	
~~~sh
cd "work/project_01"

grep -i "*.proj" "info.txt"
~~~  

&nbsp;&nbsp;**12.2. Batch file**  
- **FIND _\<options\>_ "%strings%" "%file_paths%"**  
%file_paths%에 위치한 파일들에서 %strings%와 일치하는 문자열을 찾습니다. 이때, %file_paths%가 지정되지 않으면 Prompt에서 입력되거나 다른 명령에서 파이프( | )된 텍스트에서 찾습니다. 아래는 함께 사용할 수 있는 옵션입니다.
    - /V : 지정한 문자열 %strings%가 없는 줄을 표시합니다.
    - /C : 지정한 문자열 %strings%가 있는 줄 수만을 표시합니다.
    - /N : 지정한 문자열 %strings%가 있는 각 줄 앞에 줄 번호를 붙입니다.
    - /I : 대소문자를 구별하지 않고 찾습니다.
    - /OFF 또는 /OFFLINE : 오프라인 속성 세트 파일을 건너뛰지 않습니다.

~~~batch
CD "work/project_01"

FIND /I "*.proj" "info.txt"
~~~  