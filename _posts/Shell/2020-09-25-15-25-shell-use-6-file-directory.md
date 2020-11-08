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
<br>   
&nbsp;&nbsp;**1.1. Shell script**  
- **echo _\<content\>_ > \"${file_path}\"**  
\"${file_path}\" 위치에 _\<content\>_ 를 내용으로 하는 파일을 생성합니다.   
<br>  
- **mkdir _\<option\>_ \"${new_directory_path}\"**  
\"${new_directory_path}\" 위치에 Directory를 생성합니다.   

    |**Option**|**설명**|
    |:----|:----|
    |-p|필요시 _\<option\>_ 부분에 명시할 수 있는 옵션입니다. \"${new_directory_path}\"에 존재하지 않는 Directory가 포함된 경우 중간에 존재하지 않는 Directory도 함께 생성합니다.|

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
<br>  
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
<br>
&nbsp;&nbsp;**2.1. Shell script**  
- **rm _\<options\>_ \"${path}\"**  
\"${path}\"에 위치하는 File 또는 Directory를 삭제합니다. 필요시 아래와 같은 _\<option\>_ 을 사용할 수 있습니다.    

    |**Option**|**설명**|
    |:----|:----|
    |-r|Diretory 삭제시 하위의 내용을 먼저 삭제합니다. Recursive|  
    |-i|삭제시 삭제할 것인지에 대해 묻습니다.|
    |-f|삭제에 관련된 메세지를 보여주지 않고 강제로 삭제합니다. Force| 
    |-v|삭제되는 내용을 모두 출력합니다.|
    
- **rmdir _\<option\>_ \"${directory_path}\"**  
\"${directory_path}\"에 위치하는 Directory를 삭제합니다. **rm**과 다르게 Directory만 삭제합니다. 필요시 아래와 같은 _\<option\>_ 을 사용할 수 있습니다.  

    |**Option**|**설명**|
    |:----|:----|
    |-p|삭제하려는 Directory의 상위 Directory가 비어있는 경우 함께 삭제합니다.| 
    |-r|Diretory 삭제시 하위의 내용을 먼저 삭제합니다. Recursive|
	
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

    |**Option**|**설명**|
    |:----|:----|
    |/P|각 파일을 삭제하기 전에 삭제를 확인하는 메시지를 표시합니다.|
    |/F|읽기 전용 파일을 강제 삭제합니다.|
    |/S|지정된 파일을 모든 하위 디렉터리에서 삭제합니다. 만약, 확장 기능을 사용한다면 찾지 못하는 파일이 아닌 지워진 파일을 보여줍니다.|
    |/Q|자동 모드에서는 글로벌 와일드카드에서 삭제할 것인지 묻지 않습니다.|
    |/A|특성을 기준으로 삭제할 파일을 지정합니다.|

아래는 필요시 명시할 수 있는 _\<attributes\>_ 입니다.

|**Attribute**|**설명**|
|:----|:----|
|/R|읽기 전용 파일|
|/S|시스템 파일|
|/H|숨김 파일|
|/A|보관할 파일|
|/I|콘텐츠가 인덱싱되지 않은 파일|
|/L|재분석 지점|
|/O|오프라인 파일|
|-|부정을 뜻하는 접두사|

- **RMDIR _\<option\>_ \"%directory_path%\"**  
- **RD _\<option\>_ \"%directory_path%\"**  
\"%directory_path%\"에 위치한 Directory를 삭제합니다. 필요시 아래와 같은 _\<option\>_ 을 사용할 수 있습니다.

    |**Option**|**설명**|
    |:----|:----|
    |/S|지정된 Directory 자체와 그 안의 모든 File 및 Directory를 삭제합니다. Directory Tree를 삭제하는 데 사용합니다.|
    |/Q|조용한 모드로 Directory Tree를 삭제하는 데 문제가 없으면 다시 묻지 않습니다.|

~~~batch
DEL /S /F "work_info.txt"
RMDIR /S /Q "work"
~~~  

###### 3. 복사  
&nbsp;File, Directory를 복사합니다.  
<br>  
&nbsp;&nbsp;**3.1. Shell script**  
- **cp _\<option\>_ \"${source_path}\" \"${destination_path}\"**  
\"${source_path}\"에서 \"${destination_path}\"로 복사합니다.  
<br>  
- **cp _\<option\>_ \"${source_file_paths}\" \"${destination_directory_path}\"**    
\"${source_file_paths}\"에 명시된 파일들을 \"${destination_directory_path}\" 위치의 폴더로 복사합니다. 여러 개의 파일을 한번에 복사할 수 있습니다. 필요시 아래와 같은 _\<option\>_ 을 사용할 수 있습니다.  

    |**Option**|**설명**|
    |:----|:----|
    |-a<br>\--archive|아카이브 파일. 원본 파일의 속성 및 링크 정보를 그대로 유지하면서 복사합니다. '-dR \--preserve=all'와 동일합니다.|
    |\--attributes-only|파일 정보는 복사하지 않고 속성만 복사합니다.|
    |\--backup=_\<control\>_|복사될 위치에 이미 파일이 존재하는 경우 Backup 파일을 생성합니다. 인수를 사용하지 않을 수도 있습니다.|
    |-b|'\--backup'과 비슷하지만 인수를 허용하지 않습니다.|
    |\--copy-contents|재귀시 특수 파일들의 내용을 복사합니다.|
    |-d|'\--no-dereference_'와 '\--preserve=links'와 같습니다. 즉, 복사될 위치에 Symbolic Link, Hard Link, Junction과 같은 Link관련 정보가 있다면 유지하여 복사합니다.|
    |-f<br>\--force|복사될 위치에 존재하는 파일을 열 수 없다면 삭제 후 복사합니다. 대신 '-n' 옵션 사용시 무시됩니다.|
    |-i<br>\--interactive|덮어쓰기 전 Prompt에 확인 메세지를 출력합니다. 먼저 사용한 '-n' 옵션을 Override합니다.|
    |-H|원본의 command-line Symbolic Link를 따릅니다.|
    |-l<br>\--link|복사하는 대신 Hard Link로 대채합니다.|
    |-L<br>\--dereference|항상 원본의 Symbolic Link를 따릅니다.|
    |-n<br>\--no-clobber|복사될 위치에 파일이 존재하면 덮어쓰지 않습니다. 먼저 사용한 '-i' 옵션을 Override합니다.|
    |-P<br>\--no-dereference|절대 원본의 Symbolic Link를 따르지 않습니다.|
    |-p|'\--preserve=mode,ownership,timestamps' 와 동일합니다.|
    |\--preserve=_\<attributes\>_|지정한 속성을 보존합니다. 이때, 속성을 여러 개 입력이 가능하며 따로 입력하지 않아도 됩니다.<br>- 기본 값 : mode, ownership, timestamps<br>- 사용 가능한 속성 : context, links, xattr, all|
    |\--no-preserve=_\<attributes\>_|지정한 속성을 보존하지 않습니다. 이때, 속성을 여러 개 입력 가능합니다.|
    |\--parents|Directory 하위의 전체 원본 파일 이름을 사용합니다.|
    |-R<br>\--recursive|재귀적으로 복사합니다. 폴더와 하위에 있는 파일과 폴더들을 모두 복사합니다.|
    |\--reflink=_\<when\>_|Clone/Copy-On-Write 복사본을 제어합니다.|
    |\--remove-destination|복사될 위치에 존재하는 파일을 열기 전에 삭제합니다. '\--force' 와 대비됩니다.|
    |\--sparse=_\<when\>_|부족한 파일의 생성을 제어합니다.|
    |\--strip-trailing-slashes|명시된 원본 인수들에서 후행 슬래시( / )를 제거합니다.|
    |-s<br>\--symbolic-link|복사하는 대신 Symbolic Link를 생성합니다.|
    |-S<br>\--suffix=_\<suffix\>_|일반적인 Backup의 접미사를 Override합니다.|
    |-t<br>\--target-directory=_\<directory\>_|모든 원본 인수들을 _\<directory\>_ 로 복사합니다.|
    |-T<br>\--no-target-directory|복사될 위치의 파일을 일반 파일로 취급합니다.|
    |-u<br>\--update|복사하려는 원본 파일이 복사될 위치의 파일보다 최신이거나 복사될 위치에 파일이 없을 때만 복사합니다.|
    |-v<br>\--verbose|복사 상태에 대해 출력합니다.|
    |-x<br>\--one-file-system|이 파일 시스템을 유지합니다.|
    |-Z|복사될 위치의 파일의 SELinux security context를 기본값으로 설정합니다.|
    |\--context=_\<CTX\>_|'-Z' 와 같거나 CTX가 지정된 경우 SELinux나 SMACK security context를 CTX로 설정합니다.|

~~~sh
SOURCE_DIRECTORY_PATH="`pwd`/work/project_01"
DESTINATION_DIRECTORY_PATH="`pwd`/work/project_02"
cp -r -f ${DIRECTORY_PATH} ${DESTINATION_DIRECTORY_PATH}

cp -b fruits.txt animals.txt ${DESTINATION_DIRECTORY_PATH}
~~~  

&nbsp;&nbsp;**3.2. Batch file**  
- **COPY _\<option\>_ _\<file_encoding\>_ \"%source_paths%\" _\<file_encoding\>_ \"%destination_path%\" _\<file_encoding\>_**   
_\<file_encoding\>_ 은 따로 설정하지 않으면 기본적으로 ASCII 입니다. 아래는 사용할 수 있는 Encoding 입니다.

    |**Encoding**|**설명**|
    |:----|:----|
    |/A|ASCII 파일|
    |/B|Binary 파일|

\"%source_paths%\"에 명시된 파일 또는 폴더들을 \"%destination_path%\"로 복사합니다. 여러 개를 한번에 복사할 수 있습니다.   
필요시 아래와 같은 _\<option\>_ 을 사용할 수 있습니다.  

|**Option**|**설명**|
|:----|:----|
|/D|대상 파일이 암호화 없이 만들어지도록 허용합니다.|
|/V|새 파일이 정상적으로 쓰여지는지 확인합니다.|
|/N|이름이 8자보다 길거나 확장자가 3자보다 긴 경우 짧은 파일 이름이 있으면 그 이름을 사용합니다.|
|/Y|\"%destination_path%\"에 이미 파일이나 폴더가 존재하면 확인 메세지 없이 덮어씁니다. COPYCMD 환경 변수에 이미 지정되어 있습니다.|
|/-Y|\"%destination_path%\"에 이미 파일이나 폴더가 존재하면 복사 전에 덮어쓸 것인지 확인 메세지를 출력합니다.|
|/Z|'다시 시작 가능 모드'로 파일을 복사합니다. 복사 도중에 네트워크가 단절되면 복사가 중단되고 네트워크가 다시 연결되어 복사가 가능해지면 다시 복사를 시작합니다. 약한 네트워크에서 사용됩니다.|
|/L|\"%source_paths%\"에 위치한 파일이나 폴더가 기호화된 링크(Symbolic Link, Hard Link, Junction)인 경우 복사하는 대신 기호화된 링크를 생성합니다. 기호화된 링크가 가리키는 실제 원본을 복사합니다.|  

- **XCOPY _\<option\>_ \"%source_paths%\" \"%destination_path%\"**   
\"%source_paths%\"에서 \"%destination_path%\"로 복사합니다. **COPY**의 확장 형태로 아래와 같은 다양한 옵션을 사용할 수 있습니다.

    |**Source Option**|**설명**| 
    |:----|:----|
    |/A|원본의 아카이브 속성을 그대로 복사합니다. 기본값은 Y이므로 사용됩니다.|  
    |/H|숨김 파일과 시스템 파일도 복사합니다.|  
    |/D:_\<date_format\>_|_\<date_format\>_ 에 지정된 날짜 이후에 변경된 파일을 복사합니다. 따로 _\<date_format\>_ 을 설정하지 않으면 \"%destination_path%\"에 있는 파일 혹은 폴더의 날짜보다 최신의 날짜인 원본들만 복사합니다.|  
    |/E|비어있는 폴더를 포함하여 재귀적으로 복사합니다. 비어 있는 경우를 포함하여 폴더와 하위에 있는 파일과 폴더들을 모두 복사합니다. '/T'를 수정하는 데 사용할 수 있습니다.|  
    |/EXCLUDE:_\<exclude_list_file_paths\>_|전체 경로나 일부 경로의 이름의 리스트로 구성된 파일을 설정하여 복사시 \"%source_paths%\"와 일부 일치하면 제외됩니다. _\<exclude_list_file_paths\>_ 에 여러 개의 파일을 명시할 수 있습니다. 예를 들어, exclude list에 .txt와 /work/ 가 있다면 work 폴더 내의 모든 파일을 제외하고 확장자가 .txt인 모든 파일도 제외합니다.|  
    |/M|아카이브 속성이 설정된 원본을 복사하고 아카이브 속성을 해제합니다. 일반 백업을 만들때 사용됩니다. 기본값은 Y이므로 사용됩니다.|  
    |/S|비어있는 폴더를 제외하고 재귀적으로 복사합니다. 비어있는 폴더를 제외하고 폴더와 하위에 있는 파일과 폴더들을 모두 복사합니다.|  
    |/U|\"%destination_path%\"에 이미 존재하는 경우만 복사합니다. Update|
    |**Destination Option**|**설명**|
    |/I|\"%destination_path%\"가 존재하지 않거나 두 개 이상의 파일을 복사하면 폴더로 가정합니다.|
    |/K|속성을 복사합니다. 그렇지 않으면 읽기 전용 속성을 다시 설정합니다.|  
    |/N|가능하면 \"%destination_path%\"에 파일을 생성할 때 이름 8자, 확장자 3자까지인 짧은 이름을 사용합니다. 서로 다른 Format을 가진 Disk간에 복사할 때 필요할 수 있습니다. NTFS와 VFAT 같이 포맷이 다른 Disk간의 복사나 Data를 ISO9660 CDROM으로 보관하는 경우를 예로 들 수 있습니다.|  
    |/O|파일의 소유권과 ACL 정보를 복사합니다.|  
    |/R|읽기 전용 파일을 덮어씁니다.|  
    |/T|파일은 복사하지 않고 폴더 구조만 생성합니다. 이때, 빈 폴더와 하위 폴더를 포함하지 않습니다. '/E'와 함께 사용하면 빈 폴더와 하위 폴더를 포함합니다.|  
    |/X|파일의 감사(audit) 설정을 복사합니다. /O 의미|   
    |**Common Option**|**설명**|  
    |/B|원본이 기호화된 링크인 경우 복사 대신 기호화된 링크를 생성합니다.|  
    |/C|오류가 발생해도 복사를 계속합니다.|  
    |/F|복사하는 동안 \"%source_paths%\"와 \"%destination_path%\"의 전체 경로를 출력합니다.|  
    |/G|암호화 기능을 지원하지 않는 \"%destination_path%\"에 암호화된 \"%source_paths%\"를 복사할 수 있습니다.|  
    |/J|버퍼를 사용하지 않는 I/O를 사용하여 복사합니다. 매우 큰 파일에 권장합니다.|  
    |/L|복사할 파일들의 목록만 표시합니다.|  
    |/P|\"%destination_path%\" 각 파일을 생성하기 전 Prompt에 확인 메세지를 출력합니다.|  
    |/Q|복사하는 동안 파일 이름을 출력하지 않습니다.|  
    |/V|\"%destination_path%\"에 생성되는 새 파일의 확인합니다. 읽을 수 있는 지, 크기, 아니면 기록될 때 확인합니다.|  
    |/W|복사를 시작하기 전에 Prompt에 키 입력을 하라는 메세지를 출력합니다. 아무 키나 입력이 되어야 복사를 시작합니다.|  
    |/Y|\"%destination_path%\"에 이미 파일이나 폴더가 존재하면 확인 메세지 없이 덮어씁니다. COPYCMD 환경 변수에 이미 지정되어 있습니다.|  
    |/-Y|\"%destination_path%\"에 이미 파일이나 폴더가 존재하면 복사 전에 덮어쓸 것인지 확인 메세지를 출력합니다.|
    |/Z|'다시 시작 가능 모드'로 파일을 복사합니다. 복사 도중에 중단되면 가능하면 다시 복사를 시작합니다. 약한 네트워크에서 사용됩니다.|

~~~batch
SET SOURCE_DIRECTORY_PATH="%CD%/work/project_01"
SET DESTINATION_DIRECTORY_PATH="%CD%/work/project_02"
XCOPY /S /Y %SOURCE_DIRECTORY_PATH% %DESTINATION_DIRECTORY_PATH%

COPY /B fruits.txt animals.txt %DESTINATION_DIRECTORY_PATH%
~~~  

###### 4. 존재 확인  
&nbsp;File, Directory이 실제로 있는 지 확인 합니다.  
<br>
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
<br>
&nbsp;&nbsp;**5.1. Shell script**    
- **cd /"${path}\"**  
\"${path}\"의 경로로 이동합니다. 경로 대신 아래와 같은 문자를 사용하면 지정된 위치로 이동합니다.  

    |/|Root Directory로 이동합니다.|
    |\-|직전의 Directory로 이동합니다.|
    |빈 값<br>~|사용자의 Home Directory로 이동합니다.|
    |..|상위 폴더로 이동합니다.|

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

    |/D|현재 Drive까지 변경합니다.   
    |..|경로 대신 사용하면 상위 폴더로 이동합니다.

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
<br>
&nbsp;&nbsp;**6.1. Shell script**  
- **mv _\<option\>_ \"${source_path}\" \"${destination_path}\"**  
\"${source_path}\"에 위치한 파일이나 폴더의 이름을 \"${destination_path}\"로 변경하거나 \"${source_path}\"에 있는 파일이나 폴더를 \"${destination_path}\"로 옮깁니다. 아래와 같은 옵션을 사용할 수 있습니다. 이때, -f, -i, -n 옵션은 서로 상반되는 기능이기 때문에 함께 사용하면 가장 마지막에 명시된 옵션만 적용됩니다.

    |**Option**|**설명**|
    |:----|:----|
    |\--backup=_\<control\>_|복사될 위치에 이미 파일이 존재하는 경우 Backup 파일을 생성합니다. 인수를 사용하지 않을 수도 있습니다.|
    |-b|\--backup과 비슷하지만 인자를 사용하지 않습니다.|
    |-f<br>\--force|덮어쓰기 전에 Prompt에 따로 확인 메세지를 출력하지 않습니다. _\<destination_path\>_ 에 이미 존재한다면 추가 메세지없이 덮어씁니다.|
    |-i<br>\--interactive|덮어쓰기 전에 Prompt에 확인 메세지를 출력합니다.|
    |-n<br>\--no-clobber|복사될 위치에 이미 파일이 존재하는 경우 덮어 쓰지 않습니다.| 
    |\--strip-trailing-slashes|\"${source_path}\" 경로의 마지막 슬래시( / )를 제거합니다.|
    |-S<br>\--suffix=_\<suffix\>_|일반적인 Backup의 접미사를 Override합니다.|
    |-t<br>\--target-directory=_\<directory\>_|${source_path} 에 위치한 모든 파일이나 폴더를 지정한 _\<directory\>_ 로 이동합니다.|
    |-T<br>\--no-target-directory|복사될 위치의 파일을 일반 파일로 취급합니다.|
    |-u<br>\--update|복사하려는 원본 파일이 복사될 위치의 파일보다 최신이거나 복사될 위치에 파일이 없을 때만 복사합니다.|
    |-v<br>\--verbose|복사 상태에 대해 출력합니다.|
    |-Z<br>\--context|복사될 위치의 파일의 SELinux security context를 기본값으로 설정합니다.|	

~~~sh
cd work

mv "project_01" "project_02"
~~~  

&nbsp;&nbsp;**6.2. Batch file**  
- **RENAME \"%source_file_path%\" \"%destination_file_name%\"**  
- **REN \"%source_file_path%\" \"%destination_file_name%\"**  
**RENAME**과 **REN**은 동일하게 \"%source_file_path%\"에 위치한 파일의 이름을 \"%destination_file_name%\"로 변경합니다. \"%destination_file_name%\"로 새 드라이브나 새 경로를 지정할 수 없습니다.  
<br>    
- **MOVE _\<option\>_ \"%source_paths%\" \"%destination_path%\"**  
\"%source_paths%\"에서 \"%destination_path%\"로 파일이나 폴더를 옮기고 이름을 변경합니다. \"%source_paths%\"에 여러 개의 경로를 명시하면 한번에 여러 개의 파일을 옮길 수 있습니다. \"%destination_path%\"에는 드라이브를 포함한 전체 경로나 이름을 사용하면 됩니다. 아래는 사용 가능한 옵션입니다.

    |**Option**|**설명**|
    |:----|:----|
    |/Y|\"%destination_path%\"에 이미 파일이나 폴더가 존재하면 확인 메세지 없이 덮어씁니다.|  
    |/-Y|\"%destination_path%\"에 이미 파일이나 폴더가 존재하면 복사 전에 덮어쓸 것인지 확인 메세지를 출력합니다.|

~~~batch
CD work

MOVE "project_01" "project_02"

CD project_02
REN info.txt info_prev.txt
~~~  

###### 7. File과 Directory 목록 확인  
&nbsp;현재 경로의 File과 Directory 목록을 확인합니다.  
<br>  
&nbsp;&nbsp;**7.1. Shell script**  
- **ls _\<option\>_ _\<path\>_**  
기본적으로 현재 디렉토리 기준으로 파일에 대한 정보를 나열합니다. 옵션으로 -cftuvSUX나 \--sort가 지정되지 않은 경우 알파벳순으로 항목을 정렬합니다. 아래는 사용할 수 있는 옵션입니다.  

    |**Option**|**설명**|
    |:----|:----|
    |-a<br>\--all|. 으로 시작하는 숨겨진 파일이나 폴더를 무시하지 않습니다.|
    |-A<br>\--almost-all|. 과 .. 을 묵시적으로 나열하지 않습니다.|  
    |\--author|-l 옵션과 함께 사용되며 각 파일의 작성자를 출력합니다.|
    |-b<br>\--escape|비출력 문자(Nongraphic Characters)에 대한 C-style Escapes를 출력합니다.|
    |\--block-size=_\<size\>_|크기를 출력하기 전에 크기의 단위를 _\<size\>_ 로 조정합니다. 예를 들어, \--block-size=M 이면 MB(1,048,576 bytes)단위로 출력됩니다.|
    |-B<br>\--ignore-backups|~ 로 끝나는 항목은 묵시적으로 나열하지 않습니다.|
    |-c|-lt 옵션과 함께 사용하면 ctime( 파일 상태 정보의 마지막 수정 시간 )별로 정렬하여 표시합니다. -l 옵션과 함께 사용하면 ctime을 출력하고 이름별로 정렬하거나 ctime과 최신순으로 정렬합니다.|
    |-C|열(column)별로 항목을 나열합니다.|
    |\--color=_\<when\>_|출력 색상을 설정합니다. _\<when\>_이 생략된 경우 기본적으로 always로 설정되어 있으며 그 외, auto 나 never 를 설정할 수 있습니다.|
    |-d<br>\--directory|내용을 제외한 폴더들만 나열합니다.|
    |-D<br>\--dired|Emacs' dired mode용으로 디자인된 출력을 생성합니다.|
    |-f|정렬하지 않습니다. -aU 활성화, -ls \--color 비활성화됩니다.|
    |-F<br>\--classify|항목에 indicator( */=>@\| )를 추가합니다.|
    |\--file-type|항목에 indicator( */=>@\| )를 추가하지만 '\*'을 추가하지 않습니다.|
    |\--format=_\<word\>_|format 설정을 합니다.(-x : across, -m : commas, -x : horizontal, -l : long, -1 : single-column, -l : verbose, -C : vertical)|
    |\--full-time|-l \--time-style=full-iso 와 같습니다.|
    |-g|-l와 같지만 소유자를 나열하지 않습니다.|
    |\--group-directories-first|파일보다 폴더 그룹을 먼저 나열합니다. \--sort 옵션을 사용하여 확장할 수 있지만 \--sort=none (-U) 을 사용하면 그룹화가 비활성화됩니다.|
    |-G<br>\--no-group|나열된 목록이 긴 경우 그룹 이름을 출력하지 않습니다.|
    |-h<br>\--human-readable|-l 또는 -s와 함께 사용되며 사람이 읽을 수 있는 크기로 출력됩니다.(예 : 1K, 234M, 2G)|
    |\--si|-l 또는 -s와 함께 사용되며 사람이 읽을 수 있는 크기로 출력되지만 1024가 아닌 1000의 제곱을 사용합니다.|
    |-H<br>\--dereference-command-line|명령줄에 나열된 Symbolic Link를 따라갑니다.|
    |\--dereference-command-line-symlink-to-dir|폴더를 가리키는 각 명령줄의 Symbolic Link를 따라갑니다.| 
    |\--hide=_\<pattern\>_|Shell _\<pattern\>_ 과 일치하는 묵시적인 항목은 나열하지 않습니다. -a나 -A 옵션에 의해 재정의됩니다.|
    |\--hyperlink=_\<when\>_|파일 이름에 하이퍼링크를 생성합니다.  _\<when\>_ 에는 'always', 'auto', 'never' 값 설정이 가능하며 따로 입력하지 않으면 기본값인 always로 설정됩니다.|
    |\--indicator-style=_\<word\>_|_\<word\>_ 스타일의 indicator가 진입명(Entry Name)에 추가됩니다. _\<word\>_ 의 기본값은 none이며, slash (-p), file-type (\--file-type), classify (-F) 를 사용할 수 있습니다.|
    |-i<br>\--inode|각 파일의 인덱스 번호를 출력합니다.|    
    |-I<br>\--ignore=_\<pattern\>_|Shell _\<pattern\>_ 과 일치하는 묵시적인 항목을 나열하지 않습니다.|
    |-k<br>\--kibibytes|Disk 사용시 1024 Byte 블록이 기본값입니다.|
    |-l|긴 목록 형식을 사용합니다.|
    |-L<br>\--dereference|Symbolic Link에 대한 파일 정보를 표시할 때, Link 자체보다 Link가 참조하는 파일에 대한 정보를 표시합니다.| 
    |-m|,(쉼표)로 구분된 항목 목록으로 너비를  채웁니다.|
    |-n<br>\--numeric-uid-gid|-l과 비슷하지만 숫자로된 User와 Group ID를 나열합니다.|
    |-N<br>\--literal|진입명(Entry Name)을 출력합니다.|
    |-o|-l과 비슷하지만 Group 정보를 나열하지 않습니다.|
    |-p<br>\--indicator-style=slash|/(Slash)를 추가하여 폴더를 표시합니다.|
    |-q<br>\--hide-control-chars|비출력 문자(Nongraphic Characters)는 ?로 대체하여 출력합니다.|
    |\--show-control-chars|비출력 문자(Nongraphic Characters)를 있는 그대로 출력합니다. 프로그램이 'ls'이 아니고 출력이 터미널이 아닌 경우 기본값입니다.|
    |-Q<br>\--quote-name|진입명(Entry Name)을 큰따옴표(")로 감쌉니다.|
    |\--quoting-style=_\<word\>_|진입명(Entry Name)에 _\<word\>_ 스타일의 인용을 사용합니다. _\<word\>_ 에는 literal, locale, shell, shell-always, shell-escape, shell-escape-always, c, escape 을 사용할 수 있습니다.|
    |-r<br>\--reverse|역순으로 정렬합니다.|
    |-R<br>\--recursive|재귀적으로 하위 폴더까지 나열합니다.|
    |-s<br>\--size|각 파일의 할당된 크기를 Block 단위로 출력합니다.|
    |-S|파일의 크기를 기준으로 정렬합니다. 가장 큰 크기의 파일이 가장 먼저 나옵니다. (파일 크기별 내림차순)|
    |\--sort=_\<word\>_|이름 대신 설정한 _\<word\>_ 기준으로 정렬합니다. _\<word\>_ 에는 none (-U), size (-S), time (-t), version (-v), extension (-X) 를 사용할 수 있습니다.|
    |\--time=_\<word\>_|-l과 함께 사용되며, 기본적으로 수정된 시간 대신 _\<word\>_ 를 사용한 시간을 표시합니다. atime이나 access나 use (-u), ctime이나 status (-c), 또는 \--sort=_\<time\>_ (최신순으로 정렬)을 명시한 경우 시간을 정렬 키로 사용합니다.|
    |\--time-style=_\<style\>_|-l과 함께 사용되며, _\<style\>_ 을 사용하여 시간을 표시합니다. _\<style\>_ 에는 full-iso, long-iso, iso, locale, 또는 +_\<format\>_ 을 사용할 수 있습니다. _\<format\>_ 은 날짜와 같이 해석되며, _\<style\>_ 앞에 'posix-' 가 붙으면 POSIX locale 외부에서만 적용됩니다.|
    |-t|수정된 시간을 기준으로 최신순으로 정렬합니다.|
    |-T<br>\--tabsize=_\<clos\>_|8 대신 각 _\<cols\>_ 에 설정한 값으로 Tab stop을 가정합니다.|
    |-u|-lt 와 함께 사용시 access time을 기준으로 정렬 및 표시합니다. -l 과 함께 사용시 이름 기준으로 정렬하고 access time을 표시합니다. 그 외 access time으로 가장 최신순으로 정렬합니다.|
    |-U|정렬하지 않고 폴더 순서대로 항목들을 나열합니다.|
    |-v|문자열 내의 버전과 같은 숫자를 기준으로 자연스럽게 정렬합니다.|
    |-w<br>\--width=_\<cols\>_|_\<cols\>_ 값으로 출력 너비를 설정합니다. 0이면 제한이 없다는 의미입니다.|
    |-x|열 대신 행별로 항목을 나열합니다.|
    |-X|항목의 확장자에 따라 알파벳 기준으로 정렬합니다.|
    |-Z<br>\--context|각 파일의 보안 Context를 출력합니다.|
    |-1|한 줄당 하나의 파일을 나열합니다. -q나 -b와 함께 '\n'을 사용하면 안됩니다.|  
    
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

    |**Option**|**설명**|
    |:----|:----|
    |/A:_\<attributes\>_|_\<attributes\>_ 에 지정한 속성을 가진 파일을 출력합니다. 아래는 _\<attributes\>_ 에 사용 가능한 파일 속성입니다.<br>&nbsp;&nbsp;D : 디렉터리<br>&nbsp;&nbsp;R : 읽기 전용 파일<br>&nbsp;&nbsp;H : 숨김 파일<br>&nbsp;&nbsp;A : 보관할 파일<br>&nbsp;&nbsp;S : 시스템 파일<br>&nbsp;&nbsp;I : 콘텐츠가 인덱싱되지 않은 파일<br>&nbsp;&nbsp;L : 재분석 지점<br>&nbsp;&nbsp;O : 오프라인 파일<br>&nbsp;&nbsp;- : _\<attributes\>_ 앞에 붙이면 해당 파일 속성을 제외합니다.|
    |/B|최소 포맷을 사용합니다. 머리말 정보나 요약 없음|
    |/C|파일 크기에 천 단위 구분 기호를 표시합니다.  기본적으로 설정되므로 구분 기호를 표시하지 않으려면 /-C를 사용해야 합니다.|
    |/D|가로 목록과 동일하지만 파일 목록을 세로로 정렬하여 보여 줍니다.|
    |/L|소문자를 사용합니다.|
    |/N|파일 이름이 제일 오른쪽에 오도록 새로운 긴 목록 포맷을 사용합니다.|
    |/O:_\<sortorder\>_|_\<sortorder\>_ 기준으로 파일을 정렬하여 출력합니다. 아래는 _\<sortorder\>_ 에 설정할 수 있는 정렬 기준입니다.<br>&nbsp;&nbsp;N : 이름순(알파벳순)<br>&nbsp;&nbsp;S : 크기순(가장 작은 항목부터)<br>&nbsp;&nbsp;E : 확장명순(알파벳순)<br>&nbsp;&nbsp;D : 날짜/시간순(가장 오래된 항복부터)<br>&nbsp;&nbsp;G : 그룹 디렉터리 먼저<br>&nbsp;&nbsp;- : _\<sortorder\>_ 앞에 붙이면 역순으로 정렬합니다.|  
    |/P|출력된 내용으로 Console 창 화면이 가득차면 잠깐 멈춥니다.|
    |/Q|파일 소유자를 출력합니다.|
    |/R|파일의 대체 데이터 스트림을 표시합니다.|
    |/S|지정한 폴더와 그 하위 폴더까지 함께 출력합니다.(재귀적)|
	
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
<br>    
&nbsp;&nbsp;**8.1. Shell script**  
- **ln _\<options\>_ "${source_path}" "${link_name}"**  
현재 경로에 ${source_path}를 ${link_name}으로 Link 생성합니다. 기본적으로 Hard link를 생성하므로 Symbolic Link를 생성하려면 \--symbolic 옵션과 함께 사용해야 합니다. 또한, 생성할 Link 위치에 Link든 실제 폴더나 파일이든 이미 대상이 있으면 안됩니다. Hard Link를 생성할 때 각 ${source_path}는 반드시 존재해야 합니다. Symbolic Link는 임의의 문자열을 포함할 수 있으며 이후 상대적 링크가 상위 폴더와의 관계에 의해 해석됩니다. 아래는 함께 사용할 수 있는 옵션입니다.

    |**Option**|**설명**|
    |:----|:----|
    |\--backup=_\<control\>_|복사될 위치에 이미 파일이 존재하는 경우 Backup 파일을 생성합니다. 인수를 사용하지 않을 수도 있습니다.|
    |-b|'\--backup'과 비슷하지만 인수를 허용하지 않습니다.|
    |-d <br> -F <br> \--directory|SuperUser가 Hard link 폴더에 접근 시도할 수 있도록 허용합니다. SuperUser라 하더라도 시스템 제한 사항으로 인해 실패할 수 있습니다.|
    |-f <br> \--force|Link가 생성될 위치에 이미 존재하는 파일이나 폴더를 삭제 후 Link를 생성합니다.|
    |-i <br> \--interactive|Link가 생성될 위치에 존재하는 파일이나 폴더를 삭제할 때 Prompt에 확인 메세지를 출력합니다.|
    |-L <br> \--logical|Symbolic Link인 ${source_path}의 역참조합니다.|
    |-n <br> \--no-dereference|Symbolic Link를 생성할 ${source_path}가 폴더인 경우 ${link_name}를 일반 파일로 취급합니다.|
    |-P <br> \--physical|Symbolic Link에 직접 Hard Link를 생성합니다.|
    |-r <br> \--relative|Link 위치에 상대적인 Symbolic Link를 생성한다.|
    |-s <br> \--symbolic|Hard Link 대신 Symbolic Link를 생성합니다.|
    |-S <br> \--suffix=_\<suffix\>_|일반적인 Backup 접미사를 재정의합니다. _\<suffix\>_ 를 설정한 경우 해당 값으로 재정의합니다.|
    |-t <br> \--target-directory=_\<directory\>_|Link를 생성할 Directory를 지정합니다. _\<directory\>_ 에 Link 생성할 Directory를 명시하면 됩니다.|
    |-T <br> \--no-target-directory|${link_name}을 항상 일반 파일로 취급합니다.|    

<br>  
- **rm _\<options\>_ "${link_path}"**  
${link_path}에 위치한 Link를 제거합니다. 아래는 함께 사용할 수 있는 옵션입니다.

    |**Option**|**설명**|
    |:----|:----|
    |-f <br> \--force|인자와 존재하지 않는 파일을 무시하며 Prompt에 출력하지 않습니다.|
    |-i|Link를 제거하기 전 매번 Prompt에 확인 메세지를 출력합니다.|
    |-I|3개 이상의 파일 혹은 재귀적으로 삭제하기 전에 Prompt에 확인 메세지를 한번 출력합니다. 대부분의 실수를 방지하면서 -i 를 사용하는 것보다 덜 귀찮습니다.|
    |\--interactive=_\<when\>_|_\<when\>_ 설정에 따라 Prompt에 출력합니다. _\<when\>_ 에 설정 가능한 값은 never, once ( -l ), always ( -i ) 이며 기본값은 always 입니다.|
    |\--one-file-system|계층 구조를 재귀적으로 삭제할 때, 해당 명령줄 인자와 다른 파일 시스템의 폴더는 삭제하지 않고 건너뜁니다.|
    |\--no-preserve-root|'/'를 특별하게 취급하지 않습니다.|
    |\--preserve-root|'/'를 삭제하지 않습니다. 기본값입니다.|
    |-r <br> -R <br> \--recursive|폴더와 하위에 있는 파일과 폴더들을 재귀적으로 삭제합니다.|
    |-d <br> \--dir|비어 있는 폴더들을 삭제합니다.|

~~~sh
cd "shortcut"

ln -s "work/project_01" "p01"
rm -i "p00_test"
~~~  

&nbsp;&nbsp;**8.2. Batch file**  
- **MKLINK _\<options\>_ "%LINK_NAME%" "%SOURCE_PATH%"**  
현재 경로에 %SOURCE_PATH%를 %LINK_NAME%으로 Link 생성합니다. 필요에 따라 아래와 같은 옵션을 사용할 수 있습니다.  

    |**Option**|**설명**|
    |:----|:----|
    |/D|%SOURCE_PATH%에 대한 Link를 생성합니다. 기본값은 파일 바로 가기 링크입니다.|
    |/H|바로 가기 링크 대신 하드 링크를 만듭니다.|
    |/J|디렉터리 교차점을 만듭니다.|

- **RMDIR _\<options\>_ "%LINK_PATH%"**    
%LINK_PATH%에 위치한 Link를 제거합니다. 필요에 따라 아래와 같은 옵션을 사용할 수 있습니다.  

    |**Option**|**설명**|
    |:----|:----|
    |/S|지정된 폴더와 그 하위의 폴더와 파일들을 모두 제거합니다. 폴더 구조를 일괄 제거하는 데 사용됩니다.|
    |/Q|조용한 모드로, /S로 제거하는데 문제가 없으면 다시 확인 메세지를 출력하지 않습니다.|

~~~batch
CD "shortcut"

MKLINK /D "work/project_01" "p01"
RMDIR /S /Q "p00_test"
~~~  

###### 9. 비교  
&nbsp;두 개의 File이나 Directory를 비교합니다.  
<br>
&nbsp;&nbsp;**9.1. Shell script**  
- **cmp _\<options\>_  ${file_path_1} ${file_path_2}**  
${file_path_1}과 ${file_path_2}에 위치한 두 개의 파일을 Byte 단위로 비교합니다. 종료 값은 두 파일이 같으면 0, 다르면 1, 문제가 있으면 2입니다. 아래는 함께 사용할 수 있는 옵션입니다.

    |**Option**|**설명**|
    |:----|:----|
    |-b <br> \--print-bytes|다른 Bytes를 출력합니다.|
    |-i <br> \--ignore-initial=_\<skip\>_|비교할 두 파일의 시작부분에서 _\<skip\>_ Bytes만큼 건너뛰고 비교를 시작합니다. _\<skip\>_ 값은 지정하지 않으면 기본적으로 0며, Byte 대신 KB, K, MB, M, GB, G, T, P, E, Z, Y 와 같은 단위를 사용할 수 있으며 반드시 명시해야합니다.|
    |-i <br> \--ignore-initial=_\<skip_1\>_:_\<skip_2\>_|비교할 ${file_path_1} 시작부분에서는 _\<skip_1\>_ Bytes만큼, ${file_path_2} 시작부분에서는 _\<skip_2\>_ Bytes만큼 건너뛰고 비교를 시작합니다. 비교할 파일마다 건너 뛸 Bytes를 따로 지정할 수 있습니다. _\<skip_1\>_ 과 _\<skip_2\>_ 의 값은 지정하지 않으면 기본적으로 0입니다.|
    |-l <br> \--verbose|다른 Byte 값과 번호를 출력합니다.|
    |-n <br> \--bytes=_\<limit\>_|최대로 _\<limit\>_ Bytes만큼만 비교합니다.|
    |-s <br> \--quiet <br> \--silent|모든 표준 출력을 하지 않습니다.|  

- **comm _\<options\>_ ${file_path_1} ${file_path_2}**  
정렬된 ${file_path_1}과 ${file_path_2}에 위치한 두 개의 파일을 한 줄씩 비교합니다. 비교시 'LC_COLLATE'에 지정된 규칙을 따릅니다. 아래는 함께 사용할 수 있는 옵션입니다.

    |**Option**|**설명**|
    |:----|:----|
    |-1|첫 번째 열을 출력하지 않습니다. ${file_path_1}에 위치한 파일의 고유한 Line을 출력하지 않으므로 파일을 비교하여 ${file_path_2}에만 있는 고유한 내용과 공통된 내용만 출력합니다.|
    |-2|두 번째 열을 출력하지 않습니다. ${file_path_2}에 위치한 파일의 고유한 Line을 출력하지 않으므로 파일을 비교하여 ${file_path_1}에만 있는 고유한 내용과 공통된 내용만 출력합니다.|
    |-3|세 번째 열을 출력하지 않습니다. 비교한 두 파일의 공통된 내용을 출력하지 않고 각 파일들의 고유한 내용만 출력합니다. 기본값입니다.|
    |\--check-order|모든 입력줄이 페어링이 가능하더라도 입력이 바르게 정렬되었는 지 확입니다.| 
    |\--nocheck-order|입력이 바르게 정렬되어 있는 지 확인하지 않습니다.|
    |\--output-delimiter=_\<str\>_|분리 문자열 _\<str\>_ 을 사용하여 열을 분리하여 출력합니다.|
    |\--total|요약하여 출력합니다.|
    |-z <br> \--zero-terminated|줄 구분문자(Line delimiter)가 newline(\n, \r, \n\r)이 아니라 NUL 입니다.|

- **diff _\<options\>_ ${path_1} ${path_2}**  
${path_1}과 ${path_2}에 위치한 파일이나 폴더를 한 줄씩 비교합니다. 종료 값은 두 파일이 같으면 0, 다르면 1, 문제가 있으면 2입니다. 아래는 함께 사용할 수 있는 옵션입니다.  

    |**Option**|**설명**|
    |:----|:----|
    |\--normal|기본값이며, 일반 비교를 출력합니다.|
    |-q <br> \--brief|파일들이 다를 때에만 알려줍니다.|
    |-s <br> \--report-identical-files|두 파일이 같을 때에 알려줍니다.|
    |-c <br> -C _\<num\>_ <br> \--context=_\<num\>_|_\<num\>_ 의 기본 값은 3이며, 복사된 Context의 줄을 출력합니다.|
    |-u <br> -U _\<num\>_ <br> \--unified=_\<num\>_|_\<num\>_ 의 기본 값은 3이며, 통합된 Context의 줄을 출력합니다.|
    |-e <br> \--ed|ED Script를 출력합니다.|
    |-n <br> \--rcs|RCS 포맷이 다르면 출력합니다.|
    |-y <br> \--side-by-side|두 열이 다르면 출력합니다.|
    |-W <br> \--width=_\<num\>_|_\<num\>_ 의 기본 값은 130이며, _\<num\>_ 까지의 열만 출력합니다.|
    |\--left-column|공통 줄의 왼쪽 열만 출력합니다.|
    |\--suppress-common-lines|공통 줄을 출력하지 않습니다.|
    |-p <br> \--show-c-function|각 변경 사항이 어떤 C 함수에 있는 지 표시합니다.|
    |-F <br> \--show-function-line=_\<re\>_|_\<re\>_ 값과 일치하는 가장 최근의 줄만 표시합니다.|
    |\--label _\<label\>_|파일의 이름과 타임스탬프 대신 _\<label\>_ 를 사용합니다. 반복 가능합니다.|
    |-t <br> \--expand-tabs|출력에서 Tab을 공백문자로 확장합니다.|
    |-T <br> \--initial-tab|Tab을 미리 준비해서 Tab을 정렬시킵니다.|
    |\--tabsize=_\<num\>_|Tab이 _\<num\>_ 값이면 모든 열 출력을 멈춥니다. _\<num\>_ 의 기본 값은 8입니다.|
    |\--suppress-blank-empty|빈 출력 줄 전에 공백이나 Tab을 표시하지 않습니다.
    |-l <br> \--paginate|출력을 'pr'로 전달하여 페이지를 매깁니다.|
    |-r <br> \--recursive|하위 폴더들에서도 재귀적으로 비교합니다.|
    |\--no-dereference|Symbolic Link를 따라가지 않습니다.|
    |-N <br> \--new-file|비교하려는 파일이 없는 경우 빈 파일로 취급합니다.|
    |\--unidirectional-new-file|비교하려는 첫 번째 파일이 없는 경우 빈 파일로 취급합니다.|
    |\--ignore-file-name-case|파일 이름 비교시 대소문자 구분하지 않습니다.|
    |\--no-ignore-file-name-case|파일 이름 비교시 대소문자를 구분합니다.|
    |-x <br> \--exclude=_\<pat\>_|_\<pat\>_ 과 일치하는 파일을 제외합니다.|
    |-X <br> \--exclude-from=_\<file\>_|_\<file\>_ 의 패턴과 일치하는 파일들은 제외합니다.|
    |-S <br> \--starting-file=_\<file\>_|폴더들을 비교시 _\<file\>_ 파일부터 비교를 시작합니다.|
    |\--from-file=_\<file_1\>_|_\<file_1\>_ 을 모든 피연산자(${path_2}에 명시한 경로의 파일들)와 비교합니다. _\<file_1\>_ 는 폴더가 될 수도 있습니다.|
    |\--to-file=_\<file_2\>_|_\<file_2\>_ 을 모든 피연산자(${path_1}에 명시한 경로의 파일들)와 비교합니다. _\<file_1\>_ 는 폴더가 될 수도 있습니다.|
    |-i <br> \--ignore-case|파일 내용 비교시 대소문자를 구분하지 않습니다.|
    |-E <br> \--ignore-tab-expansion|비교시 파일 내 Tab으로 인한 변경 사항을 무시합니다.|
    |-Z <br> \--ignore-trailing-space|비교시 파일 내 줄 끝에 있는 White space를 무시합니다.|
    |-b <br> \--ignore-space-change|비교시 파일 내 White space로 인한 변경 사항을 무시합니다.|
    |-w <br> \--ignore-all-space|모든 White space를 무시합니다.|
    |-B <br> \--ignore-blank-lines|비교시 파일 내 빈 칸으로만 이뤄진 줄에 의한 변경 사항을 무시합니다.|
    |-I <br> \--ignore-matching-lines=_\<re\>_|비교시 파일 내 _\<re\>_ 와 일치하는 모든 줄을 무시합니다.|
    |-a <br> \--text|모든 파일을 Text로 취급합니다.|
    |\--strip-trailing-cr|입력시 끝에 있는 Carriage Return을 제거합니다.|
    |-D <br> \--ifdef=_\<name\>_|'#ifdef _\<name\>_' 차이가 있는 병합된 파일을 출력합니다.|
    |-d <br> \--minimal|가장 작은 차이점을 찾기위해 노력합니다.|
    |\--horizon-lines=_\<num\>_|공통된 접두사와 접미사의 _\<num\>_ 줄을 유지합니다.| 
    |\--speed-large-files|큰 파일들과 많은 산재한 작은 변화들을 가정합니다.|
    |\--color=_\<when\>_|출력에 색상을 입힙니다. _\<when\>_ 값은 'never', 'always', 'auto'이며 따로 명시하지 않았을 때 기본 값은 'auto'입니다.|
    |\--palette=_\<palette\>_|\--color 옵션이 활성화되어있을 때 사용할 색상을 설정합니다. _\<palette\>_ 는 ,(Colon)으로 구분된 Terminfo Capabilities 목록입니다.|  
    |\--GTYPE-group-format=_\<gfmt\>_|GTYPE 포맷의 입력 그룹을 _\<gfmt\>_ 포맷으로 처리합니다. GTYPE는 'old', 'new', 'unchanged', 'changed'입니다.|    
    |\--line-format=_\<lfmt\>_|모든 입력 줄을 _\<lfmt\>_ 포맷으로 처리합니다. LTYPE는 'old', 'new', 'unchanged'입니다.|
    |\--LTYPE-line-format=_\<lfmt\>_|LTYPE 포맷의 입력 줄을 _\<lfmt\>_ 포맷으로 처리합니다.|  

~~~sh
cd "work"

comm "project_01/info.txt" "project_02/info.txt"
diff "project_01" "project_02"
~~~  

&nbsp;&nbsp;**9.2. Batch file**  
- **COMP %file_path_1% %file_path_2% _\<options\>_**  
%file_path_1%과 %file_path_2%에 위치한 파일들을 비교합니다. %file_path_1%, %file_path_2%의 파일 경로에 와일드 카드를 사용하면 두 개 이상의 파일도 비교할 수 있습니다. 아래는 함께 사용할 수 있는 옵션입니다.  

    |**Option**|**설명**|
    |:----|:----|
    |/D|다른 부분을 십진 형식으로 표시합니다.|
    |/A|다른 부분을 ASCII 문자로 표시합니다.|
    |/L|다른 부분에 대한 행 번호를 표시합니다.|
    |/N=_\<number\>_|각 파일에서 지정한 번호인 _\<number\>_만큼의 행만을 비교합니다.|
    |/C|ASCII 문자에 대해 대/소문자를 구별하지 않고 비교합니다.|
    |/OFF _\<line\>_|오프라인 속성 세트 파일을 건너뛰지 않습니다.|
    |/M|다른 파일을 비교할지 여부를 묻는 메시지를 표시하지 않습니다.|

- **FC _\<options\>_ %file_path_1% %file_path_2%**  
%file_path_1%과 %file_path_2%에 위치한 파일 또는 파일의 집합을 비교하고 다른 점을 화면에 출력합니다. 아래는 함께 사용할 수 있는 옵션입니다.  

    |**Option**|**설명**|
    |:----|:----|
    |/B|이진 모드에서 비교합니다. 이진 모드로 비교하는 경우 다른 옵션을 사용할 수 없습니다.|
    |/A|연속적으로 차이가 있는 부분의 첫 번째 줄과 마지막 줄만 표시합니다.|
    |/C|대/소문자를 구별하지 않습니다.|
    |/L|ASCII 파일로 취급하고 비교합니다.|
    |/LBn|연속적으로 차이가 있는 부분의 최대 줄의 수를 지정합니다.|
    |/OFF _\<line\>_|오프라인 속성 세트 파일을 건너뛰지 않습니다.|
    |/N|ASCII 문자로 비교 중 행 번호를 표시합니다.|
    |/T|탭을 공백으로 확장하지 않습니다.|
    |/U|Unicode 파일로 취급하고 비교합니다.|
    |/W|비교 중 빈 공간(탭과 공백)을 압축합니다.|
    |/nnnn|같지 않은 줄 다음에 연속적으로 같아야 하는 줄의 수를 지정합니다.|

~~~batch
CD "work"

COMP "project_01/info.txt" "project_02/info.txt"
FC "project_01" "project_02"
~~~  

###### 10. 정렬    
&nbsp;File 또는 표준 입력을 정렬하여 출력합니다.  
<br>
&nbsp;&nbsp;**10.1. Shell script**  
- **sort _\<options\>_ ${file_paths}**  
명시한 모든 ${file_paths}에 위치한 파일들의 연결을 정렬하여 표준 출력으로 씁니다. ${file_paths} 가 없거나 - 인 경우 표준 입력으로 읽습니다. 아래는 함께 사용할 수 있는 옵션입니다.

    |**Option**|**설명**|
    |:----|:----|
    |-b <br> \--ignore-leading-blanks|선행 공백을 무시합니다.|
    |-d <br> \--dictionary-order|공백이나 알파벳과 숫자로 구성된 문자만 고려합니다.|
    |-f <br> \--ignore-case|대소문자 구분하지 않습니다.|
    |-g <br> \--general-numeric-sort|일반 숫자 값에 따라 비교하여 정렬합니다.|
    |-i <br> \--ignore-nonprinting|인쇄 가능한 문자만 고려합니다.|
    |-M <br> \--month-sort|'월' 기준으로 비교합니다. (월을 모르는 경우) < 'JAN' < 'DEC' 순으로 정렬됩니다.|
    |-h <br> \--human-numeric-sort|사람이 읽을 수 있는 숫자를 비교합니다. 예로 2K, 1G|
    |-n <br> \--numeric-sort|문자로 된 숫자 값에 따라 비교하여 정렬합니다.|
    |-R <br> \--random-sort|임의로 정렬하지만 동일한 키들을 그룹화합니다. shuf(1) 참조|
    |\--random-source=_\<file\>_|_\<file\>_ 에서 임의의 Bytes를 가져옵니다.|
    |-r <br> \--reverse|비교 결과의 반대로 정렬합니다.|
    |-V <br> \--version-sort|텍스트 내 자연적인 숫자(Version)에 따라 정렬합니다.|
    |\--sort=_\<word\>_|_\<word\>_ 에 따라 정렬합니다. _\<word\>_ 에는 일반 숫자(general-numeric) -g, 사람이 읽을 수 있는 숫자(human-numeric) -h, 월(month) -M, 숫자(numeric) -n, 랜덤(random) -R, 버전(version) -V 를 사용합니다.|
    |\--batch-size=_\<nmerge\>_|임시 파일들을 최대로 사용하기 위해 입력한 최대  _\<nmerge\>_ 를 한번에 병합합니다.|
    |-c <br> \--check <br> \--check=diagnose-first|따로 정렬하지는 않고 정렬된 입력을 확인합니다.|
    |-C <br> \--check=quiet <br> \--check=silent|-c 와 비슷하지만 잘못된 첫 번째 줄은 보고하지 않습니다.|
    |\--compress-program=_\<prog\>_|지정한 압축 프로그램 _\<prog\>_ 를 사용하여 임시로 압축합니다. _\<prog\>_ -d로 사용하면 압축 해제를 합니다.| 
    |\--debug|정렬을 사용한 줄부분에 주석을 달고 stderr의 의심스러운 사용에 대하여 경고합니다.|
    |\--files0-from=_\<f\>_|지정한 파일 _\<f\>_ 에서 NUL로 끝나는 이름으로 지정된 파일들을 입력을 읽습니다. 만약 _\<file\>_ 가 -이면 표준 입력에서 이름을 읽습니다.|
    |-k <br> \--key=_\<keydef\>_|키를 통해 정렬합니다. _\<keydef\>_ 는 F[.C][OPTS][,F[.C][OPTS]] 로 위치와 형식을 제공합니다. 시작과 중지 위치에 대한 것이면 F는 Field 번호, C는 Field에서 문자의 위치입니다. F와 C가 모두 1이면 정치 위치는 기본적으로 줄 끝에 위치합니다. -t와 -b가 모두 유효하지 않으면 Field 내 문자는 앞의 WhiteSpace부터 시작하여 계산합니다. OPTS는 해당 Key에 대한 전역 순서 옵션을 재정의하는 하나 이상의 단일 문자 순서 옵션 [bdfgiMhnRrV]이다. 만약 Key가 없는 경우 전체 줄을 Key를 사용합니다. \--debug 옵션을 사용하여 잘못된 Key 사용을 진단할 수 있습니다.|
    |-m <br> \--merge|이미 정렬된 파일들을 병합합니다. 따로 정렬하지 않습니다.|
    |-o <br> \--output=_\<file\>_|표준 출력 대신 _\<file\>_ 로 결과를 출력합니다.|
    |-s <br> \--stable|마지막 비교를 비활성화하여 정렬을 안정화합니다.|
    |-S <br> \--buffer-size=_\<size\>_|Main Memory Buffer에 사용할 _\<size\>_ 를 지정합니다. _\<size\>_ 는 곱셈, Memory, 등의 접미사를 사용할 수 있습니다. 접미사 K가 기본값이며 그 외 b, M, G, T, P, E, Z, Y 등을 사용할 수 있습니다.|
    |-t <br> \--field-separator=_\<sep\>_|공백을 _\<sep\>_ 을 사용하여 대체합니다.|
    |-T <br> \--temporary-directory=_\<dir\>_|_\<dir\>_ 에 위치한 폴더를 임시로 사용합니다. $TMPDIR 이나 /tmp 는 사용할 수 없으며 옵션을 여러 개 사용하여 여러 개의 폴더를 지정할 수 있습니다.| 
    |\--parallel=_\<n\>_|동시에 정렬을 진행할 수를 _\<n\>_ 로 설정합니다.|
    |-u <br> \--unique|-c 와 함께 사용하면 엄격하게 순서를 확인하고 -c와 함께 사용하지 않으면 동일한 실행 중 첫 번째만 출력합니다.|
    |-z <br> \--zero-terminated|줄 구분자가 NewLine이 아니라 NUL 입니다.|

~~~sh
cd "work"
sort -n 3 "project_01/info.txt" "project_02/info.txt"
~~~  

&nbsp;&nbsp;**10.2. Batch file**    
- **SORT _\<optoins\>_ %file_paths%**  
%file_paths% 위치에 있는 파일을 정렬합니다. 따로 지정한 %file_paths%가 없으면 표준 입력이 정렬되며 %file_paths%를 지정하면 같은 파일을 리디렉션하는 것보다 빠릅니다. 아래는 함께 사용할 수 있는 옵션입니다.

    |**Option**|**설명**|
    |:----|:----|
    |/+_\<n\>_|각 비교를 시작할 문자 번호 _\<n\>_ 을 지정합니다. _\<n\>_ 미만의 줄은 다른 줄보다 먼저 정렬됩니다. 기본적으로 비교는 각 줄의 첫 번째 문자에서 시작합니다. 예로, /+3 이면 각 비교가 각 줄의 3번쨰 문자부터 시작되어야 한다는 것을 나타냅니다.|
    |/L _\<locale\>_ <br> /LOCALE _\<locale\>|시스템의 기본 Locale을 지정한 _\<locale\>_ 로 재정의합니다. "C" locale은 가장 빠른 조합 순서를 생성하며 현재 유일한 대안입니다. 해당 정렬은 항상 대소문자를 구분하지 않습니다.|
    |/M _\<kilobytes\>_ <br> /MEMORY _\<kilobytes\>_|정렬시 사용할 Main Memory의 양(Kilobytes)을 설정합니다. 메모리 크기는 항상 최소 160 KB로 제한됩니다. Main memory의 가용 크기에 상관하지 않고 지정한 메모리 크기만큼 정확한 양을 정렬에 사용합니다. 보통 메모리 크기를 설정하지 않으면 최상의 퍼포먼스를 얻을 수 있습니다. 기본적으로 기본 최대 메모리 크기에 맞을 때, 한 번의 한 번의 pass(임시 파일 없음)로 정렬이 되고 그렇지 않으면 두번의 pass(부분적을 정렬된 Data가 임시 파일에 저장)에서 모든 파일의 정렬과 병합에 사용되는 메모리 크기의 양이 같도록 정렬이 진행된다. 기본 최대 메모리 크기는 입출력 파일이 Main memory에서 사용 가능한 기본 최대 메모리 크기는 90%이고 입출력이 파일인 경우 Main memory의 90%, 그 외의 경우 45%까지 사용가능합니다.|
    |/REC _\<char_count\>_ <br> /RECORD_MAXIMUM _\<char_count\>_|Record 내 최대 문자의 수를 _\<char_count\>_ 로 설정합니다. 기본 값은 4,096, 최대 값은 65,535 입니다.|
    |/R <br> /REVERSE|정렬을 역순으로 변경합니다.|
    |/T _\<temp_directory_path\>_ <br> /TEMPORARY _\<temp_directory_path\>_|정렬 작업에 필요한 데이터가 Main memory를 초과하는 경우 해당 내용을 저장할 Directory 경로를 지정합니다. 기본 경로는 시스템 임시 디렉터리입니다.|
    |/O _\<output_file_path\>_ <br> /OUTPUT _\<output_file_path\>_|정렬된 데이터를 저장할 파일 경로를 지정합니다. 지정하지 않으면 표준 출력 파일에 기록됩니다.  출력 파일을 지정하면 같은 파일을 리디렉션하는 것보다 빠릅니다.|
	
~~~batch
CD "work"
SORT /R "project_01/info.txt" "project_02/info.txt"
~~~  

###### 11. 화면당 출력  
&nbsp;File의 Perusal Filter로 File을 Prompt 한 화면에 한번씩 나눠서 출력합니다.  
<br>
&nbsp;&nbsp;**11.1. Shell script**   
- **more _\<options/>_ ${file_path}**  
Terminal에서 보기 위한 파일 열람 필터로 ${file_path} 위치의 파일을 출력합니다. 아래는 함께 사용할 수 있는 옵션입니다. 

    |**Option**|**설명**|
    |:----|:----|
    |-d|경고 대신 도움 문구를 표시합니다.|
    |-f|화면의 줄보다는 논리적인 계산을 합니다.|
    |-l|양시을 Feed 한 후 일시 중지를 하지 않도록 제한합니다.|
    |-c|Scroll, 텍스트 표시, 줄 끝 정리를 하지 않습니다.|
    |-p|Scroll, 화면 지움, 텍스트 표시를 하지 않습니다.|
    |-s|여러 개의 빈 줄을 하나의 줄로 처리합니다.|
    |-u|밑줄을 제한합니다.|
    |\- _\<number\>_|화면당 줄 수를 설정합니다.|
    |+_\<number\>_|설정한 줄 번호부터 파일에서 화면으로 표시하기 시작합니다.|
    |+/_\<string\>_|파일에서 설정한 _\<string\>_ 문자열과 일치하는 부분부터 화면으로 표시하기 시작합니다.|  

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

    |**Option**|**설명**|
    |:----|:----|
    |/C|페이지를 표시하기 전에 화면을 지웁니다.|
    |/P|FF(form-feed) 문자를 확장합니다.|
    |/S|러 개의 빈 줄을 하나의 빈 줄로 바꿉니다.|
    |/Tn|탭을 n개(기본값은 8)의 공백으로 바꿉니다.|
    |+_\<n\>_|첫 번째 파일을 _\<n\>_ 줄에서부터 출력합니다.|
    |/E|확장 기능을 사용할 수 있게 합니다.|
    ||**확장 기능**|
    |P _\<n\>_|다음 _\<n\>_ 줄을 표시합니다.|
    |S _\<n\>_|다음 _\<n\>_ 줄을 건너뜁니다.|
    |F|다음 파일을 표시합니다.|
    |Q|마칩니다.|
    |=|줄 번호를 표시합니다.|
    |?|도움말을 표시합니다.|
    |[공백]|다음 페이지를 표시합니다.|
    |[Enter]|다음 줄을 표시합니다.|

~~~batch
CD "work/project_01"
MORE /C < "info.txt"
~~~  

###### 12. 파일 안에서 문자열 찾기   
&nbsp;File에서 문자열을 찾습니다.  
<br>
&nbsp;&nbsp;**12.1. Shell script**   
- **grep _\<options\>_ ${pattern} ${file_paths}**  
명시된 ${file_paths} 각 파일에서 ${pattern}과 일치하는 문자열을 찾습니다. ${file_paths}가 '-'이면 표준 입력을 읽고 ${file_paths}가 없을 때 재귀적이면 '.' 재귀적이지 않으면 '-'로 읽습니다. ${file_paths}가 2개 이하이면 -h로 가정합니다. 줄을 선택하면 종료 상태는 0, 그렇지 않으면 1, 에러가 발생하였을 때 -q 옵션이 사용되지 않았으면 2입니다. 아래는 함께 사용할 수 있는 옵션입니다.

    |**Option**|**설명**|
    |:----|:----|
    |-E <br> \--extended-regexp|${pattern}에 확장된 정규 표현식을 사용할 수 있습니다.|
    |-F <br> \--fixed-strings|${pattern}은 Newline으로 구분된 문자열들로 구성됩니다.|
    |-G <br> \--basic-regexp|${pattern}은 기본 정규 표현식을 사용합니다. 기본 값입니다.|
    |-P <br> \--perl-regexp|${pattern}은 Perl 정규 표현식입니다.|
    |-e <br> \--regexp=_\<pattern\>_|옵션에 설정한 _\<pattern\>_ 과 일치|
    |-f <br> \--file=_\<file\>_|_\<file\>_ 에서 ${pattern}으로 사용할 값을 가져옵니다.|
    |-i <br> \--ignore-case|대소문자를 무시합니다.|
    |-w <br> \--word-regexp|${pattern}이 전체 단어에 일치하도록 강제 설정합니다.|
    |-x <br> \--line-regexp|${pattern}이 전체 줄에 일치하도록 강제 설정합니다.|
    |-z <br> \--null-data|Data 줄의 끝은 Newline이 아닌 0 byte로 끝납니다.|
    |-s <br> \--no-messages|오류 메세지를 표시하지 않습니다.|
    |-v <br> \--invert-match|일치하지 않은 줄을 선택합니다.|
    ||**출력 제어**| 
    |-m <br> \--max-count=_\<num\>_|선택한 _\<num\>_ 번째 줄 이후로 멈춥니다.|
    |-b <br> \--byte-offset|줄마다 Byte offset과 함께 출력합니다.|
    |-n <br> \--line-number|줄마다 줄 번호와 함께 출력합니다.|
    |\--line-buffered|모든 줄에 출력 Flush|
    |-H <br> \--with-filename|줄마다 파일 이름과 함께 출력합니다.|
    |-h <br> \--no-filename|출력시 파일 이름 접두사를 표시하지 않습니다.|
    |\--label=_\<label\>_|표준 입력 파일 이름 접두사 대신 _\<label\>_ 을 사용합니다.|
    |-o <br> \--only-matching|줄에서 ${pattern}과 일치하는 부분만 표시합니다.|
    |-q <br> \--quiet <br> \--silent|일반 출력을 모두 표시하지 않습니다.|
    |\--binary-files=_\<type\>_|Binary 파일들을 _\<type\>_ 으로 가정합니다. _\<type\>_ 은 'binary', 'text', 'without_match' 로 구성됩니다.|
    |-a <br> --text|\--binary-files=text 와 동일합니다.|
    |-I|\--binary-files=without-match 와 동일합니다.|
    |-d <br> \--directories=_\<action\>_|폴더들의 처리 방법으로 _\<action\>_ 은 'read', 'recurse', 'skip'을 사용 할 수 있습니다.|
    |-D <br> \--devices=_\<action\>_|FIFO, Socket, Device의 처리 방법으로  _\<action\>_ 은 'read', 'skip'을 사용 할 수 있습니다.|
    |-r <br> \--recursive|--directories=recurse 와 동일합니다.|
    |-R <br> \--dereference-recursive|-r 과 비슷하지만 모든 Symbolic Link를 따릅니다.|
    |\--include=_\<file_pattern\>_|_\<file_pattern\>_ 와 일치하는 파일들만 검색합니다.|
    |\--exclude=_\<file_pattern\>_|_\<file_pattern\>_ 와 일치하는 파일이나 폴더는 생략됩니다.|
    |\--exclude-from=_\<file\>_|_\<file\>_ 에서 file pattern과 일치하는 파일은 생략됩니다.|
    |\--exclude-dir=_\<pattern\>_|_\<pattern\>_ 과 일치하는 폴더는 생략됩니다.|
    |-L <br> \--files-without-match|선택하지 않은 줄은 ${file_paths} 이름만 출력합니다.|
    |-l <br> \--files-with-matches|선택한 줄은 ${file_paths} 이름만 출력합니다.|
    |-c <br> \--count|파일당 선택한 줄의 수만 출력합니다.|
    |-T <br> \--initial-tab|필요한 경우 탭 정렬을 합니다.|
    |-Z <br> \--null|파일 이름 뒤에 0 byte를 출력합니다.|
    |-B <br> \--before-context=_\<num\>_|선행 Context의 _\<num\>_ 번째 줄을 출력합니다.|
    |-A <br> \--after-context=_\<num\>_|후행 Context의 _\<num\>_ 번째 줄을 출력합니다.|
    |-C <br> \--context=_\<num\>_|출력 Context의 _\<num\>_ 번째 줄을 출력합니다.|
    |-NUM|\--context=_\<num\>_ 과 동일합니다.|
    |\--color=_\<when\>_ <br> \--colour=_\<when\>_|일치하는 문자열에 Marker를 사용하여 강조 표시를 합니다. _\<when\>_ 에는 'always', 'never', 'auto' 를 사용할 수 있으며 빈 값으로 두면 기본 값인 'auto'입니다.|
    |-U <br> \--binary|EOL(MSDOS/Windows)에서 CR 문자를 제거하지 않습니다.|

~~~sh
cd "work/project_01"

grep -i "*.proj" "info.txt"
~~~  

&nbsp;&nbsp;**12.2. Batch file**  
- **FIND _\<options\>_ "%strings%" "%file_paths%"**  
%file_paths%에 위치한 파일들에서 %strings%와 일치하는 문자열을 찾습니다. 이때, %file_paths%가 지정되지 않으면 Prompt에서 입력되거나 다른 명령에서 파이프( | )된 텍스트에서 찾습니다. 아래는 함께 사용할 수 있는 옵션입니다.

    |**Option**|**설명**|
    |:----|:----|
    |/V|지정한 문자열 %strings%가 없는 줄을 표시합니다.|
    |/C|지정한 문자열 %strings%가 있는 줄 수만을 표시합니다.|
    |/N|지정한 문자열 %strings%가 있는 각 줄 앞에 줄 번호를 붙입니다.|
    |/I|대소문자를 구별하지 않고 찾습니다.|
    |/OFF <br> /OFFLINE|오프라인 속성 세트 파일을 건너뛰지 않습니다.|

~~~batch
CD "work/project_01"

FIND /I "*.proj" "info.txt"
~~~  

###### 13. 파일이나 폴더 찾기    
&nbsp;지정한 경로에서 파일이나 폴더를 찾습니다.  
&nbsp;&nbsp;**13.1. Shell script**
- **find _\<options\>_ "${path}" _\<expression\>_**  
지정한 ${path} 경로에서 표현식 _\<expression\>_ 에 맞는 파일, 폴더를 나열합니다. ${path}가 지정되지 않은 경우 현재 Directory 경로를 기본 값으로 사용합니다. 명령줄에 ``- ( ) , !`` 문자와 해당 문자가 시작하는 첫 번째 인자까지 검색을 진행할 파일 또는 Directory 이름으로 취급하며 그 뒤에 따라오는 인자는 검색할 대상에 대한 표현식 \<expression\>으로 취급합니다. 검색에 영향을 주는 옵션은 마지막 경로 바로 뒤에 설정하지만 Symbolic Link의 처리를 제어하는 -H, -L, -P 옵션들은 첫 번째 경로 바로 앞에 설정합니다. -H, -L, -P 옵션이 하나 이상 설정된 경우 명령줄 내 마지막에 오는 옵션만 적용됩니다. -H와 -L 옵션이 지정되지 않으면 -P가 기본값입니다. 아래는 해당 옵션의 자세한 설명입니다.

|**Option**|**설명**|
|:----|:----|
|-P|**find**의 기본 값으로 Symbolic link를 절대 따르지 않습니다. Symbolic link인 파일을 **find**로 검색하거나 출력할 때 사용되는 정보는 Symbolic link 자체의 속성으로부터 가져옵니다.|
|-L|Symbolic link를 사용합니다. 파일들에 대해 **find**로 검색하거나 출력할 때 사용되는 정보는 Symbolic link 자체가 아니라 Symbolic link가 가리키는 파일의 속성으로부터 가져옵니다. 이때, Symbolic link가 끊겼거나 파일을 검사할 수 없는 경우는 제외합니다.<br>이 옵션의 사용은 ``-noleaf``를 의미하고 나중에 -P 옵션을 사용해도 -noleaf 옵션은 여전히 유효합니다.<br>-L 옵션이 유효할때 검색 중 하위 Directory에 대한 Symbolic link를 찾으면 Symbolic link가 가리키는 하위 Directory가 검색되고 -type 부분은 Symbolic link가 끊긴 경우를 제외하고 Symbolic link 자체보다 Symbolic link가 가리키는 파일 형식과 항상 일치합니다. -lname과 -ilanme 구문은 항상 false를 반환합니다.|
|-H|명령줄 인자를 제외한 Symbolic link를 따르지 않습니다. **find**로 검색하거나 출력할 때 사용되는 정보는 Symbolic link 자체의 속성으로부터 가져옵니다. 하지만 명령줄에 지정한 파일이 Symbolic link이고 확인이 가능한 상태이면 Symbolic link가 가리키는 파일의 속성으로 부터 정보를 가져옵니다. 즉, 기본적으로 Symbolic link가 가리키는 파일을 검사할 수 없는 경우 예비로 Symbolic link 자체를 사용합니다. -H 옵션이 유효하고 명령줄에 지정된 경로 중에 Directory에 대한 Symbolic link이 있는 경우 Directory 하위의 내용을 검색합니다. 만약 ``-maxdepth 0``을 사용하면 Directory 하위 검색이 불가능합니다.

표현식 _\<expression\>_ 의 기본은 -print입니다. -H나 -L 옵션이 설정되어 있을 때 ``-newer`` 인자로 나열된 Symbolic link는 참조되지 않고 Symbolic link가 가리키는 파일에서 Timestamp를 사용합니다. 이는 ``-anewer``와 ``-cnewer``에서도 동일하게 적용됩니다. ``-follow`` 옵션은 Symbolic link가 나타나는 시점에 적용이 다르지만 -L 옵션과 비슷합니다. -L 옵션은 사용되지 않고 -follow 옵션만 사용되는 경우 명령줄에서 -follow 옵션 앞의 Symbolic link는 참조되며 뒤의 Symbolic link는 참조되지 않습니다. 표현식은 아래와 같이 Operator, Option, Test, Action으로 구성됩니다.  

- Operator : 쉼표( , ) 연산자는 파일 시스템의 계층을 한번만 지나가지만 여러 Type을 검색할 때 유용합니다. 이때, -fprintf를 사용하여 일치하는 다양한 항목들을 여러 다른 파일로 출력할 수 있습니다. 아래는 우선순위가 감소하는 순서대로 나열되어있는 Operator입니다.    

    |**Operator**|**설명**|
    |:----|:----|
    |( \<expr\> )|강제로 우선 처리됩니다.|
    |! \<expr\>|\<expr\>이 false일 때 true입니다.|
    |-not \<expr\>|! \<expr\>과 동일하지만 POSIX와 호환되지 않습니다.|
    |\<expr_1\> \<expr_2\>|두 표현식이 한 줄에 나열된 경우 묵시적으로 'and'과 함께 결합됩니다.<br>\<expr_1\>이 false인 경우 \<expr_2\>는 따로 처리하지 않습니다. 즉, 두 표현식이 모두 true인 경우에만 true입니다.| 
    |\<expr_1\> -a \<expr_2\>|\<expr_1> \<expr_2>와 동일합니다.|
    |\<expr_1\> -and \<expr_2\>|\<expr_1> \<expr_2>와 동일하지만 POSIX와 호환되지 않습니다.|
    |\<expr_1\> -o \<expr_2\>|'OR'을 의미합니다. \<expr_1\>이 true이면 \<expr_2\>는 따로 처리하지 않습니다. 즉, 두 표현식 중 하나만 true여도 true입니다.|
    |\<expr_1\> -or \<expr_2\>|\<expr_1\> -o \<expr_2\>와 동일하지만 POSIX와 호환되지 않습니다.|
    |\<expr_1\> , \<expr_2\>|'List'. 항상 표현식 \<expr_1\>과 \<expr_2\>를 처리합니다. \<expr_1\> 값은 폐기되고 \<expr_2\> 값이 해당 전체 목록의 값이 됩니다.|

- Positional option : 항상 true입니다.
    
    |**Option**|**설명**|
    |:----|:----|
    |-daystart|24시간 전보다 오늘 시작부터 시간을 측정합니다. 명령줄에서 'Test' 옵션에만 영향을 줍니다.<br>-amin, -atime, -cmin, -ctime, -mmin, -mtime|
    |-follow|사용되지 않음. -L 로 대체되었습니다. Symbolic Link 참조를 제거합니다. -noleaf를 의미합니다. 명령줄에서 뒤에 따라오는 'Test' 옵션에만 영향을 줍니다.<br>-H나 -L 옵션을 따로 명시하지 않으면 -follow 옵션의 위치는 -newer의 동작을 변경합니다.<br>-newer의 인자로 나열된 파일들이 Symbolic Link인 경우 참조가 제거됩니다. 이는 -anewer와 -cnewer에 동일하게 적용됩니다.<br>마찬가지로 -type 옵션은 Link 자체보다 Symoblic Link가 가리키는 파일 형식과 항상 일치합니다.<br>-follow 옵션을 사용하면 -lname과 -ilname 옵션은 항상 false를 반환합니다.|
    |-L|
    |-regextype \<type\>|나중에 명령줄에서 발생하는 -regex와 -iregex 'Test'에서 이해하는 정규표현식으로 변경합니다.<br>현재 구현된 유형은 emacs(기본값), posix-awk, posix-basic, posix-egrep, posix-extended입니다.|

- Normal option : 항상 true입니다. 표현식이 처리되지 않아도 항상 영향을 미쳐 true입니다. 그렇기때문에 확실하게 하기 위해 표현식 가장 앞 부분에 명시하는 것이 좋고 그렇지 않으면 경고가 발생합니다.  

    |**Option**|**설명**|
    |:----|:----|
    |-depth|각각의 Directory 내용을 해당 Directory보다 먼저 검색합니다.|
    |-d|-depth와 같습니다. FreeBSD, NetBSD, MacOS X, OpenBSD와 호환됩니다.|
    |-maxdepth \<levels\>|명령줄 인수에 지정한 Directory Level(음이 아닌 정수)에서부터 하위 Directory Level까지 검색합니다.<br>(예) -maxdepth 0 : 명령 줄의 'Test', 'Action'만 적용됩니다.|
    |-mindepth \<levels\>|명령줄 인수에 지정한 Directory Level(음이 아닌 정수)보다 작은 Level에는 'Test'와 'Action'을 적용하지 않습니다.<br>(예) -mindepth 1 : 명령줄 인자를 제외한 모든 파일을 검색합니다.|
    |-mount|다른 파일시스템의 Directory들은 하위 내용을 검색하지 않습니다. 일부 다른 버전의 find와의 호환성 위한 -xdev의 다른 이름입니다.|
    |-noleaf|Directory에 Hard Link 수보다 하위 Directory가 2개 적다고 가정하여 최적화하지 않습니다. CD-ROM이나 MS-DOS filesystems이나 AFS volume mount points 같이 Unix Directory-Link 규약을 따르지 않는 파일시스템에서 찾을 때 필요합니다. 일반 Unix 파일시스템의 각 Directory에는 적어도 이름과 '.' 항목 이렇게 2개의 Hard link가 있고 하위 Directory가 있는 경우 각각 해당 Directory에 연결된 '..' 항목이 있습니다. **find**가 Directory를 검색할 때, Link보다 2개가 적은 하위 Directory를 검색한 후 Directory의 나머지 항목이 Directory가 아니라 Directory 구조 내 파일이라는 것을 알 수 있습니다. 파일 이름을 검색하지 않아도 되면 검색할 필요가 없으므로 검색 속도가 크게 향상됩니다.|
    |-xdev|다른 파일시스템에서 Directory를 검색하지 않습니다.|
    |-ignore_readdir_race|일반적으로 **find**가 파일을 검색할 수 없으면 에러 메세지를 발생합니다. 이 옵션을 설정하면 Directory에서 파일 이름을 검색한 시점과 실제로 파일을 검색하려고 시도하는 시점 사이에 해당 파일이 삭제되어도 에러 메세지가 발생하지 않습니다. 명령줄에 이름이 지정된 파일이나 Directory에도 적용됩니다. 명령줄이 읽을 때 옵션이 적용되므로 해당 옵션을 사용하면서 파일시스템의 일부를 검색하고 다시 옵션을 해제한 후 파일시스템의 일부를 검색할 수 없습니다. 만약, 파일시스템의 일부분만 해당 옵션을 적용하거나 적용하고 싶지 않다면 명령을 쪼개서 처리해야합니다.|
    |-noignore_readdir_race|-ignore_readdir_race 효과를 제거합니다.|
    |-warn|경고 메세지를 켭니다. 경고는 명령줄에서 사용할 때만 적용되며 Directory를 검색할 때 발생할 수 있는 조건들에는 적용되지 않습니다. tty가 표준 입력이면 -warn에 해당합니다.|
    |-nowarn|경고 메세지를 끕니다. tty가 표준 입력이 아니면 -nowarn에 해당합니다.|

- Test : true나 false를 반환합니다. **+N**은 N보다 클 때, **-N**은 N보다 작을 때, **N**은 N과 동일할 때 true입니다.  
    
    |:----|:----|
    |**Accessed**||
    |-amin \<n\>|\<n\>분 전에 마지막으로 파일에 액세스되었습니다.|
    |-anewer \<file\>|파일 \<file\>이 수정된 것보다 더 최근에 액세스되었습니다. Command line에서 -anewer 전에 -follow가 먼저 위치하는 경우 -follow 영향을 받습니다.|
    |-atime \<n\>|\<n\> * 24 시간 전에 마지막으로 파일에 액세스되었습니다.|
    |**Changed**||
    |-cmin \<n\>|\<n\>분 전에 파일의 상태가 마지막으로 변경되었습니다.|
    |-cnewer \<file\>|파일 \<file\>이 수정된 것보다 더 최근에 상태가 변경되었습니다. Command line에서 -cnewer 전에 -follow가 먼저 위치하는 경우 -follow 영향을 받습니다.|
    |-ctime \<n\>|\<n\> * 24 시간 전에 파일의 상태가 마지막으로 변경되었습니다.|
    |-empty|파일이 비어있고 일반 파일이나 Directory입니다.|
    |-false|항상 false입니다.|
    |-fstype \<type\>|파일 시스템 유형에 파일 \<type\>이 있습니다. 유효한 파일 시스템 유형은 Unix 버전에 따라 다양합니다.|
    |-gid \<n\>|파일의 숫자 그룹 ID가 \<n\>입니다.|
    |-group \<gname\>|파일이 \<gname\> 그룹에 속합니다. 숫자로된 그룹 ID도 허용됩니다.|
    |-ilname \<pattern\>|-lname과 비슷하지만 대소문자를 구분하지 않습니다.|
    |-iname \<pattern\>|-name과 비슷하지만 대소문자를 구분하지 않습니다.|
    |-iwholename \<pattern\>|-wholename과 비슷하지만 대소문자를 구분하지 않습니다.|
    |-inum \<n\>|파일에 inode 번호 \<n\>이 있습니다.|
    |-ipath \<pattern\>|-path와 비슷하지만 대소문자를 구분하지 않습니다.|
    |-iregex \<pattern\>|-regex와 비슷하지만 대소문자를 구분하지 않습니다.|
    |-links \<n\>|파일이 \<n\> 링크를 가지고 있습니다.|
    |-lname \<pattern\>|파일은 내용이 Shell Pattern의 패턴과 일치하는 내용을 가진 심볼릭 링크입니다. Slash( / )나 마침표( . )를 특별하게 취급하지 않습니다.|
    |**Modified**||
    |-mmin \<n\>|파일의 데이터가 \<n\>분 전에 마지막으로 수정되었습니다.|
    |-mtime \<n\>|파일의 데이터가 \<n\> * 24시간 전에 마지막으로 수정되었습니다.| 
    |-name \<pattern\>|파일 이름이 Shell Pattern의 패턴과 일치합니다. MetaCharacter( * ? [ ] )는 기본 이름의 시작부분에서 마침표( . )과 일치하지 않습니다. Directory와 그 하위 파일을 무시하려면 **-prune**을 사용하면 됩니다.|
    |-newer \<file\>|\<file\>보다 최근에 수정된 파일입니다. Command line에서 -newer 전에 -follow가 먼저 위치하는 경우 -follow 영향을 받습니다.|
    |-nouser|파일의 숫자로 된 User ID에 해당하는 사용자가 없습니다.|
    |-nogroup|파일의 숫자로 된 Group ID에 해당하는 Group이 없습니다.|
    |-path \<pattern\>|-wholename을 참조|
    |-perm \<mode\>|파일의 권한 Bit는 정확히 \<mode\>(Octal 이나 Symbolic)와 일치합니다. 정확하게 일치해야하므로 Symbolic Mode에서 사용하기 위해서는 약간 복잡한 Mode 문자열 지정을 해야합니다.|
    |-perm -\<mode\>|파일에 대한 모든 Permission bit mode가 설정되어 있습니다. Symbolic Mode에서 사용 가능하고 사용하고자 할 때 보통 사용되는 형태입니다. Symbolic Mode를 사용할 때 u, g, o 를 지정해야합니다.|
    |-perm +\<mode\>|파일에 대한 Permission bit mode가 설정되어 있습니다. Symbolic Mode에서 사용 가능한 형태이며 사용시 u, g, o를 지정해야합니다.|
    |-regex \<pattern\>|파일 이름이 정규 표현식 패턴 \<pattern\>과 일치합니다. 이때, 검색이 아니라 전체 경로에서 일치해야 합니다.
    |-wholename \<pattern\>|파일 이름이 Shell Pattern의 패턴과 일치합니다. ?e/?f 나 ?e.?f를 특별하게 취급하지 않습니다. Directory Tree에 있는 모든 파일과 폴더를 확인하지 않고 무시하려면 -prune을 사용하면 됩니다.|
    |-size \<n\>|파일이 사용하는 용량 \<n\>입니다. Indirect block을 계산하지는 않지만 실제로 할당되지 않는 Sparse File의 Block을 계산합니다.\<n\>에는 아래와 같은 접미사를 사용할 수 있습니다.<br>b : 512 byte Block으로 접미사가 없을 경우 기본 값으로 사용됩니다.<br>c : Byte<br>w : 2 Byte 단어<br>k : 킬로바이트(1024 bytes)<br>M : 메가바이트(1048576 bytes)<br>G : 기가바이트(1073741824 bytes)|
    |-true|항상 true|
    |-type \<c\>|파일의 Type이 \<c\>입니다. 아래는 \<c\>에 사용할 수 있는 값입니다.<br>b : 특수한 Block(Buffered)<br>c : 특수한 문자(Unbuffered)<br>d : 디렉토리<br>p : 명명된 Pipe(FIFO)f : 일반 파일<br>l : Symbolic Link(-L 이나 -follow 옵션은 Symbolic Link가 끊기기 전까지 유효합니다.)<br>s : Socket<br>D : door(Solaris)|
    |-uid \<n\>|파일의 숫자로 된 User ID가 \<n\>입니다.|
    |-used \<n\>|파일의 상태가 마지막으로 변경된 이후 \<n\> 일 후에 마지막으로 액세스한 파일입니다.| 
    |-user \<uname\>|파일의 소유자가 \<uname\>입니다. 이때, 숫자로된 User ID도 사용 가능합니다.|
    |-samefile \<name\>|\<name\>과 동일한 inode를 가진 파일을 가리킵니다. -L이 유효한 경우 Symbolic Link도 포함됩니다.|
    |-xtype \<c\>|파일이 Symbolic Link가 아닌 경우 -type과 동일합니다. Symbolic Link인 경우, -H나 -P 옵션이 지정되고 \<c\> 타입에 대한 Link이면 true, -L 옵션이 지정되고 \<c\>가 'l'이면 true입니다. 즉, Symbolic Link의 경우 -xtype은 -type이 확인하지 않는 파일의 타입을 확인합니다.|
    |-context \<scontext\><br>\--context \<scontext\>|파일이 Security Context \<scontext\>를 가지고 있습니다.(SELinux에서만 사용 가능합니다.)|
    |-readable|
    |-writable|
    |-executable|

- Action : 연산자를 사용하여 여러 Action을 사용할 수 있습니다. 생략된 경우 -로 가정되며 기본 Action은 표현식이 true인 경우 모든 파일을 -print하는 것입니다.  

    |**Option**|**설명**|
    |:----|:----|
    |-delete|파일들을 삭제합니다. 삭제에 성공하면 true, 실패하면 에러 메세지를 출력합니다. 해당 옵션을 사용하면 자동적으로 -depth 옵션이 활성화됩니다. 실제로 삭제하기 전에 -print -depth을 사용하여 테스트할 수 있습니다.|
    |-print0|항상 true입니다. 표준 출력으로 파일 이름 전체를 출력한 후 -print가 newline으로 사용하는 문자를 출력합니다. 보통 newline 문자를 따로 지정하지 않는 경우 null입니다. 파일 이름에 Newline이나 다른 형식의 White space가 포함된 경우라도 **find** 출력을 처리하는 프로그램에 의해 올바르게 해석되며 이는 xargs 옵션의 -0에 해당됩니다.|
    |-printf \<format\>|항상 true입니다. \ escape와 % 지시어를 해석하여 \<format\>을 표준 출력으로 출력합니다. Field 너비와 정밀도는 'printf' 함수로 지정할 수 있습니다. 많은 Field가 %d가 아닌 %s로 출력되며 이는 Flag가 정상적으로 동작하지 않는 다는 것을 의미할 수 있습니다. - Flag가 동작하면 Field를 강제로 왼쪽 정렬합니다. -print와 달리 -printf는 문자열 끝에 Newline을 추가하지 않습니다.|
    |-fprintf \<file\> \<format\>|항상 true입니다. -printf와 같지만 -fprint와 같은 파일에 출력합니다. 조건자가 일치하지 않더라도 출력 파일은 항상 생성됩니다.|
    |-print|항상 true입니다. 파일의 전체 이름과 그 끝에 Newline을 표준 출력으로 출력합니다. **find**의 출력을 다른 프로그램으로 전달하기 위해 \| 를 사용하고 파일에 개행 문자가 포함되어 있지 않을 경우 -print0 사용을 고려할 수 있습니다.|
    |-fprint0 \<file\>|항상 true입니다. -print0와 같지만 -fprint와 같은 파일에 출력합니다. 조건자가 일치하지 않더라도 출력 파일은 항상 생성됩니다.|
    |-fprint \<file\>|항상 true입니다. 전체 파일의 이름을 지정한 \<file\>에 출력합니다. **find** 실행시 파일이 없으면 생성되고 그렇지 않으면 잘립니다. 파일 이름을 '/dev/stdout'과 '/dev/stderr'로 지정하는 경우 각각 표준 출력과 표준 에러로 특수 처리됩니다. 조건자가 일치하지 않더라도 출력 파일은 항상 생성됩니다.|
    |-ls|항상 true입니다. 표준 출력으로 현재 파일 목록을 ``ls -dils`` 형식으로 나열합니다. 블록 수는 환경 변수 POSIXLY_CORRECT를 따로 지정하지 않는 한 1K이며 이 경우 512 Bytes 블록을 사용합니다.|
    |-fls \<file\>|항상 true입니다. -ls와 같지만 -fprint와 같은 파일에 출력합니다. 조건자가 일치하지 않더라도 출력 파일은 항상 생성됩니다.|
    |-prune|-depth와 함께 사용되지 않으면 true이며 파일이 Directory일때 하위를 검색하지 않습니다. -depth와 함께 사용되면 false이고 아무 영향이 없습니다.|
    |-quit|즉시 종료합니다. 하위 프로세스는 실행되지 않지만 명령줄에 지정한 경로는 더 이상 처리되지 않습니다. ``-execdir ... {} +``로 구성된 모든 명령줄이 종료되기 전에 호출되며 종료 상태는 이미 오류가 발생했는 지에 따라 0이 아닐 수 있습니다.<br>(예)find /tmp/foo /tmp/bar -print -quit<br>결과 : /tmp/foo|
    |-exec \<command\> ;|\<command\>를 실행합니다. 상태값이 0으로 반환되는 경우 true입니다. **find**에 대한 모든 인자는 ;로 구성된 인자까지 명령에 대한 인자로 취급합니다. 문자열 '{}'은 일부 **find** 버전에서처럼 인자가 단독으로 처리되지 않고 \<command\>의 인자가 적용되는 모든 현재 파일의 이름으로 대체됩니다. Shell에 의한 확장으로부터 보호하기 위해 Escape('\\')나 따옴표( " )로 감싸야 할 수 있습니다. \<command\>는 Directory에서 시작되며 지정한 \<command\>가 일치하는 각 파일에 대해 한 번씩 실행됩니다. 피할 수 없는 보안 문제가 있기때문에 -execdir 옵션을 대신 사용하는 것이 좋습니다.|
    |-exec \<command\> {} +|``-exec`` 옵션의 변형으로 선택한 파일들에 대해 지정한 명령을 실행하지만 명령줄은 마지막에 선택한 각 파일 이름을 추가하여 빌드합니다. 명령의 총 호출 수가 일치하는 파일 수보다 훨씬 적습니다. 명령줄은 ``xargs``와 거의 같은 방식으로 빌드됩니다. 명령 내에 '{}' 인스턴스 하나만 허용되며 명령은 시작 Directory에서 실행됩니다.|
    |-ok \<command\> ;|``-exec``와 같지만 표준 입력으로 사용자에게 먼저 물어봅니다. 응답이 'y'나 'Y'로 시작하지 않는 경우 명령을 실행하지 않고 바로 false를 반환합니다. 명령이 실행되는 경우 표준 입력은 /dev/null로 Redirection됩니다.|
    |-execdir \<command\> ;<br>-execdir \<command\> {} +|``-exec``와 같지만 지정된 명령은 일반적인 검색이 시작되는 Directory가 아닌 일치하는 파일이 포함된 하위 Directory에서 실행됩니다. 일치된 파일에 대한 경로를 처리하는 동안 다른 조건을 처리하는 것을 피하기 때문에 훨씬 더 안전한 명령입니다. ``-exec``와 마찬가지로 ``-execdir`의 '+'는 명령줄에서 하나 이상의 일치된 파일이 처리되도록 빌드하지만 명령을 호출할 때 같은 하위 Directory에 있는 파일들만 나열하도록 합니다. 이 옵션을 사용시 $PATH 환경 변수가 현재 Directory를 참조하지 않아야 합니다. 그렇지않으면 공격자는 -execdir 실행으로 Directory에 원하는 적당한 이름의 파일을 남겨놓아 원하는 명령어를 실행할 수 있습니다.|
    |-okdir \<command\> ;|``-execdir``와 같지만 표준 입력으로 사용자에게 먼저 물어봅니다. 응답이 'y'나 'Y'로 시작하지 않는 경우 명령을 실행하지 않고 바로 false를 반환합니다. 명령이 실행되는 경우 표준 입력은 /dev/null로 Redirection됩니다.|
    
    아래는 Action의 Print \<format\>에 사용되는 Escape와 지시어입니다. 이때, \ 뒤에 다른 문자가 있는 경우 일반 문자로 취급되어 둘 다 출력됩니다. 아래 지정된 문자의 경우 % 뒤에 붙으면 출력되지 않고 버려집니다. 그 외 다른 문자는 출력됩니다. %m, %d 지시문은 #, 0, +를 지원하며 숫자 지시문인 G, U, b, D, k, n은 지원되지 않습니다. - Format Flag가 지원되고 Field의 정렬이 오른쪽 맞춤(기본)에서 왼쪽 맞춤으로 변경됩니다.  

    |**Format Escape&지시어**|**설명**|
    |:----|:----|
    |\a|Alarm bell|
    |\b|Backspace|
    |\c|즉시 출력을 멈추고 갱신합니다|
    |\f|From feed|
    |\n|Newline|
    |\r|Carriage return|
    |\t|수평 Tab|
    |\v|수직 Tab|
    |\\ |ASCII NUL|
    |\\\\ |Backslash( \ )|
    |\\<nnn\>|ASCII code가 8진수 \<nnn\>인 문자입니다.|
    |%%|백분율 기호|
    |%a|파일의 마지막 Access 시간을 'ctime' 함수가 반환한 형식으로 표시합니다.|
    |%A\<format\>|@나 'strftime' 함수의 지시어 중 하나로 파일에 마지막으로 Access한 시간을 지정한 \<format\> Format으로 표시합니다. 아래는 \<format\>에 사용 가능한 값입니다. 일부는 시스템별 'strftime' 차이로 인해 동일하게 사용할 수 없을 수 있습니다.<br>* 시간 fields : @ seconds since Jan. 1, 1970, 00:00 GMT.<br>&nbsp;H : 시간. 00 ~ 23으로 표시<br>&nbsp;I : 시간. 01 ~ 12로 표시<br>&nbsp;k : 시간. 0 ~ 23으로 표시<br>&nbsp;l : 시간. 1 ~ 12로 표시<br>&nbsp;M : 분. 00 ~ 59으로 표시<br>&nbsp;p : 현재 지역의 AM(오전), PM(오후)<br>&nbsp;r : 12시간 기준의 시간 (예) hh:mm:ss [AM/PM]<br>&nbsp;S : 초. 00 ~ 60로 표시<br>&nbsp;T : 24시간 기준의 시간 (예) hh:mm:ss<br>&nbsp;+ : +로 분리되는 날짜와 시간. 시간은 현재 시간대(TZ 환경 변수 설정에 영향을 받음) 기준입니다. (예) 1961-06-17+18:00:04<br>&nbsp;X : 현재 지역의 시간 표시 (예) H:M:S<br>&nbsp;Z : 시간대. 시간대가 정해지지 않은 경우 빈 값입니다. (예) EDT<br>* 날짜 fields<br>&nbsp;a : 현재 지역의 축약된 요일 (예) Sun, Mon, Tue, Wed, Thu, Fri, Sat<br>&nbsp;A : 현재 지역의 요일 (예) Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday<br>&nbsp;b : 현재 지역의 축약된 월 (예) Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec<br>&nbsp;B : 현재 지역의 월 (예) January, February, March, April, May, June, July, Augst, September, October, November, December<br>&nbsp;c : 현재 지역의 날짜와 시간 (예) Sat Jun 04 18:00:04 KST 1961<br>&nbsp;d : 월 기준의 일. 01 ~ 31로 표시<br>&nbsp;D : 날짜. mm/dd/yy<br>&nbsp;h : b 와 동일<br>&nbsp;j : 년 기준의 일. 001 ~ 366로 표시<br>&nbsp;m : 월. 01 ~ 12으로 표시<br>&nbsp;U : 년 기준의 일요일부터 시작하는 주의 수. 00 ~ 53로 표시<br>&nbsp;w : 주 기준의 일. 0 ~ 6으로 표시<br>&nbsp;W : 년 기준의 월요일부터 시작하는 주의 수. 00 ~ 53로 표시<br>&nbsp;x : 현재 지역의 날짜 표시. mm/dd/yy<br>&nbsp;y : 년의 마지막 두자리. 00 ~ 99로 표시<br>&nbsp;Y : 년. 1970 ~<br>&nbsp;|
    |%b|512 bytes 블록에서 파일에 사용된 Disk 공간. Disk 공간은 파일시스템의 블록 크기의 배수로 할당되므로 보통 '%s / 1024'보다 크지만 파일이 Sparse 파일인 경우 더 작을 수도 있습니다.|
    |%c|파일의 상태가 마지막으로 변경된 시간을 'ctime' 함수가 반환한 형식으로 표시합니다.|  
    |%C\<format\>|파일의 상태가 마지막으로 변경된 시간을 지정한 \<format\> Format으로 표시합니다 %A와 동일한 Format을 사용합니다.|
    |%d|Directory 구조에서 파일의 Depth입니다. 0은 파일이 명령줄 인자입니다.|
    |%D|파일이 위치하는 장치 번호(십진수)입니다. stat 구조체의 st_dev 값입니다.|
    |%f|선행 Directory가 제거된 파일의 이름입니다. 오직 마지막 항목만 표시합니다.|
    |%F|파일이 있는 파일시스템의 유형입니다. 해당 값을 -fstype에서 사용할 수 있습니다.|
    |%g|파일의 그룹 이름이나 이름이 없는 경우 숫자로 된 그룹 ID입니다.|
    |%G|파일의 숫자로 된 그룹 ID|
    |%h|파일 이름을 제외한 선행 Directory. 파일이 현재 Directory에 있고 이름에 Slash가 없으면 . 으로 확장됩니다.|
    |%H|파일이 발견된 명령줄 인자|
    |%i|파일의 inode 번호(십진수)|
    |%k|1K 블록에서 파일에 사용된 Disk 공간. Disk 공간은 파일시스템에서 블록 크기의 배수로 할당되므로 '%s / 1024'보다 크지만 sparse 파일인 경우 더 작을 수도 있습니다.|
    |%l|Symbolic link의 객체. 파일이 Symbolic link가 아닌 경우 빈 문자열입니다.|
    |%m|파일의 권한 Bits(8진수). 대부분의 Unix 구현에서 사용하는 전통적인 번호를 사용하지만 특정 구현에서 8진수 권한 Bit를 비정상적인 순서로 사용하는 경우 실제 파일의 Mode와 %m의 출력값에 차이가 있습니다. 일반적으로 숫자 앞에 0이 표시되도록 하려면 #을 사용하면 됩니다.(%#m)|
    |%M|파일의 권한. ls의 경우 Symbolic 형식입니다. 이 지시어는 4.2.5 버전 이상에서 지원됩니다.|
    |%n|파일의 Hard Link 수|
    |%p|파일의 이름|
    |%P|파일이 발견된 명령줄 인수의 이름이 제거된 파일의 이름|
    |%s|파일의 크기(Bytes)|
    |%t|파일의 마지막 수정 시간을 'ctime' 함수에서 반환한 형태로 표시합니다.|
    |%T\<format\>|파일의 마지막 수정 시간을 지정한 \<format\> Format으로 표시합니다 %A와 동일한 Format을 사용합니다.|
    |%u|파일의 User 이름이나 User가 이름이 없는 경우 숫자로 된 User ID|
    |%U|파일의 숫자로 된 User ID|
    |%y|ls -l와 같은 파일의 타입.<br>U=알수 없는 타입(발생하면 안됨)|
    |%Y|%y와 같은 파일의 타입과 Symbolic link.<br>L=loop<br>N=nonexistent|
    |%Z|파일의 보안 Context. SELinux에서만 사용됩니다.|  

~~~sh
## 확장자가 .txt인 파일만 검색합니다.
ARRAY=$(find -depth -type f . \( -iname "*.txt" \))
for ${f} in ${ARRAY[@]} do
    echo ${f}
done
~~~

&nbsp;&nbsp;**13.2. Batch file**  
- **FOR _\<for_option\>_ %%\<element_variable_c\> IN ('DIR \<expression\> _\<dir_option\>_') DO ( ~ )**  
**DIR**을 사용하여 표현식 \<expression\>과 일치하는 파일 또는 Directory를 찾아 %%\<element_variable_c\>에 설정합니다.  

~~~batch
:: 확장자가 .txt인 파일만 검색합니다.
FOR /F %%f IN ('DIR *.txt /B /O:-D') DO (
    echo %%f
)
~~~