---
layoout: post
title:  "Golang의 환경 변수"
crawlertitle: post_golang_env_vars
date:   2021-02-28 17:19:41 +0900
categories: golang
summary: "Golang에서 사용되는 환경 변수에 대한 설명입니다."
---  
###### 1. GO111MODULE
&nbsp;GOPATH 모드나 모듈 지원(Module-aware) 모드 중 Go 명령어가 실행될 모드를 설정합니다. 기본값은 Golang 1.15 이하는 **auto**, 그 외는 **on**입니다. 
- **off** : Go 명령어는 go.mod 파일을 무시하고 GOPATH 모드로 실행됩니다.  
- **on** : 기본값. Go 명령어는 go.mod 파일이 없어도 모듈 지원(Module-aware) 모드에서 실행됩니다. 다만 모든 명령어가 go.mod 파일이 없이 실행되지는 않습니다.  
- **auto** : go.mod 파일이 현재 Directory나 상위 Directory에 있으면 모듈 지원(Module-aware) 모드로 실행됩니다. go.mod 파일이 없는 경우에도 Version Query와 함께 ``go mod``의 하위 명령과 ``go install``이 실행됩니다.  

###### [참고]  
- [Golang 환경 변수](https://golang.org/cmd/go/#hdr-Environment_variables)
