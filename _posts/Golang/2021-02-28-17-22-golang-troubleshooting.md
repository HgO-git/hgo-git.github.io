---
layoout: post
title:  "Golang 사용시 발생하는 문제 해결"
crawlertitle: post_golang_troubleshooting
date:   2021-02-28 17:22:19 +0900
categories: golang
summary: "Golang 사용시 발생하는 문제를 해결합니다."
---  
###### go: cannot find main module; see 'go help modules'  
- 상황 : Golang 설치 후 환경변수까지 설정하였으나 실행되지 않았습니다.  
- 해결 : Golang 1.16으로 업데이트 후 아래의 명령어를 입력하여 환경변수 GO111MODULE을 **auto**로 설정합니다.

~~~go
go env -w GO111MODULE=auto
~~~  


