---
layoout: post
title:  "Golang을 Shell 대체 사용"
crawlertitle: post_golang_use_shell
date:   2021-01-24 22:49:56 +0900
categories: golang
summary: "Golang의 기능으로 Shell을 대체하여 사용할 수 있습니다."
---  
###### 1. Command-line Argument  
- **var Args []string**  
명령줄 인수입니다. 0번째 인수는 실행된 프로그램명입니다.

~~~go
func main() {
	for i, arg := range os.Args {
		fmt.Printf("[%d] %s\n", i, arg)
	}
}
~~~

###### 2. 경로  
- **func Getwd() (dir string, err error)**  
현재 디렉토리에 해당하는 Root 경로의 이름을 반환합니다. Symbolic Link 때문에 여러 경로에 도달하는 경우 그 중 하나의 경로만 반환합니다.  

- **func Chdir(dir string) error**  
현재 작업 디렉토리를 전달된 dir 디렉토리로 변경합니다. 오류는 *PathError 타입입니다.  

~~~go
func main() {
	curWorkingDirectory, err := os.Getwd()
	if err != nil {
		fmt.Printf("현재 경로를 가져오지 못했습니다. : %v\n", err)
		return
	}
	fmt.Printf("현재 경로 : %s\n", curWorkingDirectory)

	if err = os.Chdir("work"); err != nil {
		fmt.Printf("경로 변경에 실패했습니다. : %v\n", err)
		return
	}
}
~~~

###### 3. Symbolic link  
- **func Symlink(oldname, newname string) error**  
oldname에 대해 newname으로 심볼릭 링크를 생성합니다. 오류는 *LinkError 타입입니다.  

- **func Lstat(name string) (FileInfo, error)**  
name에 해당하는 파일을 설명하는 FileInfo를 반환합니다. 파일이 Symbolic link인 경우 Link를 따르려고 시도하지 않으며 Symbolic Link를 설명하는 FileInfo를 반환합니다. 오류는 *PathError 타입입니다.  

~~~go
// MakeSymbolicLink : sourcePath를 원본으로 linkPath로 Symbolic link를 생성합니다.
func MakeSymbolicLink(sourcePath, linkPath string) error {
	if sourcePath == "" {
		return fmt.Errorf("Source path is empty")
	}
	if _, err := os.Stat(sourcePath); os.IsNotExist(err) {
		return err
	}

	// 상위 폴더가 없으면 생성합니다.
	linkParentDirectoryPath := filepath.Dir(linkPath)
	if info, err := os.Stat(linkParentDirectoryPath); os.IsNotExist(err) || !(info.IsDir()) {
		if err := os.MkdirAll(linkParentDirectoryPath, os.ModePerm); err != nil {
			return err
		}
	}

	// 이미 심볼릭 링크인 경우 종료합니다.
	if info, err := os.Lstat(linkPath); err == nil {
		if info.Mode()&os.ModeSymlink != 0 {
			return nil
		}
	}

	// 심볼릭 링크를 생성합니다.
	err := os.Symlink(sourcePath, linkPath)
	if err != nil {
		return err
	}

	return nil
}
~~~

###### 4. 프로그램 종료  
&nbsp;Shell script에서 실행한 Go 프로그램이 종료시 반환하는 값에 따라 성공, 오류 처리를 할 수 있습니다.

- **func Exit(code int)**  
현재 프로그램이 주어진 상태 코드로 종료되도록 합니다. 일반적으로 상태 코드 값이 0이면 성공, 그렇지 않으면 실패(오류)입니다. 프로그램은 즉시 종료되며 defer로 지연된 함수는 실행되지 않습니다.  
프로그램 이식성을 위하여 상태 코드는 [0, 125] 범위의 값을 사용합니다.  

~~~go
func main() {
	defer func() {
		panicMsg := recover()
		if resultValue != nil {
			fmt.Printf("[panic] %v", resultValue)
			os.exit(1)
		}
	}()

	fmt.Println("Success")
}
~~~

###### [참고]  
- [os 패키지](https://golang.org/pkg/os)
