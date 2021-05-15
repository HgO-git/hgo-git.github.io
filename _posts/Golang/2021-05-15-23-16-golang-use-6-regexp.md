---
layoout: post
title:  "Golang 정규표현식"
crawlertitle: post_golang_use_regexp
date:   2021-05-15 23:16:05 +0900
categories: golang
summary: "Golang에서 정규표현식을 사용합니다."
---  
###### 1. Match  
&nbsp;정규표현식 패턴과 확인할 값을 전달하여 바로 일치하는 지 확인할 수 있습니다. 간단한 정규표현식인 경우 주로 사용하며 이보다 복잡한 형태의 쿼리의 경우 Regexp 인터페이스와 Compile을 사용해야 합니다. 

- **func Match(pattern string, b []byte) (matched bool, err error)**  
byte slice인 b에 정규표현식 pattern과 일치하는 값이 포함하는 지 여부를 반환합니다.  

- **func MatchReader(pattern string, r io.RuneReader) (matched bool, err error)**  
RuneReader인 r이 반환한 텍스트에 정규표현식 pattern과 일치하는 값이 포함하는 지 여부를 반환합니다.  

- **func MatchString(pattern string, s string) (matched bool, err error)**  
문자열 s에 정규표현식 pattern과 일치하는 값이 포함하는 지 여부를 반환합니다.  

- **func QuoteMeta(s string) string**  
문자열 s에서 모든 정규식 메타문자를 Escape하는 문자열을 반환합니다. 반환된 문자열은 Literal text와 일치하는 정규식입니다.  

###### 2. Regexp  
&nbsp;Compile된 정규표현식이며 Longest와 같이 구성된 함수를 제외하고 여러 goroutine에서 동시에 사용해도 안전합니다. 일반적으로 동일한 정규표현식을 여러번 사용해야 하거나 단순 Match 함수보다 복잡한 정규표현식을 요하는 경우 사용됩니다.  
<br>  

&nbsp;**2.1. Compile**  
- **func Compile(expr string) (*Regexp, error)**  
정규식을 구문 분석하고 성공하면 텍스트와 일치하는 지 비교할 때 사용할 수 있는 Regexp 객체를 반환합니다.  
텍스트와 일치하는 지 비교할 때 regexp는 입력된 값의 맨 왼쪽에서부터 가장 먼저 일치하는 항목을 반환하고 그중에 역으로 검색시 가장 먼저 일치했을 항목을 선택합니다. 이를 Leftmost-First Matching이라 부르며 비록 역으로 검색시 비용이 발생하지 않지만 Perl, Python, 그 외 구현된 다른 언어에서 사용하는 것과 같습니다.  
POSIX Leftmost-Longest Matching의 경우 CompilePOSIX를 참조하면 됩니다.  
<br>

- **func CompilePOSIX(expr string) (*Regexp, error)**  
**Compile**과 비슷하지만 정규식을 POSIX ERE(egrep) 구문으로 제한하고 Leftmost-Longest Matching으로 변경합니다. 그에 따라, 텍스트와 일치하는 지 비교할 때 regexp는 입력된 값의 맨 왼쪽에서부터 가장 먼저 일치하는 항목을 반환하고 그 중에서 가장 긴 항목을 선택합니다. 이를 Leftmost-Longest Matching이라 부르며 초기 정규식 구현에서 사용되고 POSIX가 지정하는 것과 같습니다.  
그러나 서로 다른 하위 일치 항목을 갖는 여러개의 Leftmost-Longest 일치 항목들이 있을 수 있으며 여기서 POSIX와는 다릅니다. Leftmost-Longest 일치 항목들 중에서 이 패키지는 역으로 검색했을 때 찾은 가장 첫번째 항목을 선택하고 POSIX는 첫 번째 하위 표현식의 길이를 최대화한 다음에 두 번째을 왼쪽에서 오른쪽으로 일치 항목을 선택하도록 지정합니다.  
POSIX 규칙은 계산적으로 금지되며 잘 정의되지도 않습니다. 자세한 내용은 [https://swtch.com/~rsc/regexp/regexp2.grexp#posix](https://swtch.com/~rsc/regexp/regexp2.grexp#posix)를 참조하세요.  
<br>

- **func MustCompile(str string) *Regexp**  
**Compile**과 비슷하지만 표현식을 구문 분석할 수 없는 경우 panic이 발생합니다. 컴파일된 정규식을 갖는 글로벌 변수의 안전한 초기화를 단순화합니다.  

&nbsp;**2.2. Match**  
- **func (re *Regexp) Match(b []byte) bool**  
전달된 byte slice인 b에 정규표현식 re와 일치하는 값이 포함하는 지 여부를 반환합니다.  
<br>

- **func (re *Regexp) MatchString(s string) bool**  
문자열 s에 정규표현식 re와 일치하는 값이 포함하는 지 여부를 반환합니다.  
<br>

&nbsp;**2.3. Replace**  
- **func (re *Regexp) ReplaceAll(src, repl []byte) []byte**  
byte slice인 src에서 정규표현식 re와 일치하는 값을 전달된 byte slice인 repl로 대체하여 복사본 src를 반환합니다. repl 내부에서는 $ 기호가 확장되어 해석됩니다. 예를 들어, $1은 첫 번째 하위 일치 항목 텍스트를 나타냅니다.

###### 3. 정규표현식 작성 예  
&nbsp;Perl, Python, 그 외 구현된 다른 언어와 동일한 정규표현식 구문을 사용합니다.

~~~go
package main

import (
	"fmt"
	"os"
	"regexp"
	"strings"
)

func main() {
	defer func() {
		panicMsg := recover()
		if panicMsg != nil {
			fmt.Printf("[panic] %v", panicMsg)
		}
	}()

	var filePath string = "work/project_01/info.txt"
	var regexpPath *regexp.Regexp
	var err error

	if match, err := regexp.MatchString("([a-zA-Z0-9!@#$%^&()_-]+).txt", filePath); match && (err == nil) {
		fmt.Printf("Match *.txt : %s\n", filePath)
	}

	regexpPath, err = regexp.Compile("/([a-zA-Z0-9!@#$%^&()_-]+)/project_")
	if err != nil {
		panic(err)
	}

	if curWDPath, err := os.Getwd(); err == nil {
		var arrayFilePaths []string
		arrayFilePaths, err = GetAllFilePaths(curWDPath, true)
		if err != nil {
			return
		}

		for _, filePath := range arrayFilePaths {
			if regexpPath.MatchString(filePath) {
				fmt.Printf("%s -> %s\n", filePath, regexpPath.ReplaceAllString(filePath, "backup_work"))
			}
		}
	}
}

func GetAllFilePaths(rootDirectoryPath string, recursive bool) ([]string, error) {
	if rootDirectoryPath == "" {
		return []string{}, fmt.Errorf("Root directory path is invalid")
	}

	var openedFileInfo *os.File
	var arraySubFileInfo []os.FileInfo
	var arrayFilePaths []string
	var err error

	openedFileInfo, err = os.Open(rootDirectoryPath)
	if err != nil {
		return []string{}, err
	}
	defer openedFileInfo.Close()

	arraySubFileInfo, err = openedFileInfo.Readdir(-1)
	if err != nil {
		return []string{}, err
	}

	for _, relativePath := range arraySubFileInfo {
		if !(relativePath.IsDir()) {
			arrayFilePaths = append(arrayFilePaths, CombinePath(rootDirectoryPath, relativePath.Name()))
			continue
		}

		if recursive {
			subRelativePaths, err := GetAllFilePaths(CombinePath(rootDirectoryPath, relativePath.Name()), true)
			if (err != nil) || (len(subRelativePaths) <= 0) {
				arrayFilePaths = append(arrayFilePaths, CombinePath(rootDirectoryPath, relativePath.Name()))
			} else {
				for _, subRelativePath := range subRelativePaths {
					arrayFilePaths = append(arrayFilePaths, subRelativePath)
				}
			}
		}
	}

	return arrayFilePaths, nil
}

func CombinePath(arrayPathParts ...string) string {
	var path string

	for i := 0; i < len(arrayPathParts); i++ {
		if arrayPathParts[i] == "" {
			continue
		}

		if path == "" {
			path = arrayPathParts[i]
			continue
		}
		if strings.HasSuffix(path, "/") || strings.HasSuffix(path, "\\") {
			path = path + arrayPathParts[i]
		} else {
			path = path + "/" + arrayPathParts[i]
		}
	}

	return path
}
~~~

###### [참고]  
- [regexp 패키지](https://golang.org/pkg/regexp)
- [re2 Syntax](https://github.com/google/re2/wiki/Syntax)
