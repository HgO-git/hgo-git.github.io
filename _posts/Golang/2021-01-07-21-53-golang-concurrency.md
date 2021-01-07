---
layoout: post
title:  "Golang에서의 Concurrent programming"
crawlertitle: post_golang_concurrency
date:   2021-01-07 21:53:00 +0900
categories: golang
summary: "Golang에서의 Concurrent programming에 대한 사용법과 그 구현에 대한 설명입니다."
---  
&nbsp;**메모리를 공유하여 통신하는 대신 Channel을 이용한 통신으로 메모리를 공유합니다.**
<br>
많은 환경에서의 Concurrent programming은 공유 변수에 대한 올바른 접근을 구현하기 위해 섬세함을 요하므로 어렵습니다. Golang에서는 공유된 값이 채널을 통해 전달되고 실행되는 별도의 스레드에서 절대 공유되지 않는 다른 접근 방식을 권장합니다. 언제든지 한번에 하나의 goroutine만 값에 접근할 수 있습니다. 설계상 Data 경쟁은 발생할 수 없습니다. 하나의 CPU에서 실행되는 단일 스레드 프로그램을 고려하므로 동기화가 필요하지 않습니다. Golang의 동시성 접근은 Hoare의 Communication Sequential Processes(CSP)에서 비롯되었으나 Unix 파이프의 type-safe generalization으로 볼 수도 있습니다.  
<br>
###### 1. Goroutines  
&nbsp;기존에 사용하던 thread, coroutine, process 등의 용어가 부정확한 의미를 전달하기 때문에 goroutine이라고 합니다. 동일한 주소 공간에서 다른 goroutine과 함께 동시에 실행되는 함수로 가볍고 스택 영역 할당보다 조금 더 많은 비용이 듭니다. 작은 크기의 스택으로 시작하므로 저렴하고 필요에 따라 Heap 영역의 할당과 해제를 통해 증가시킵니다. goroutine은 여러 OS Thread로 멀티플렉싱되므로 I/O를 기다리는 동안 차단되어야 하는 경우 다른 thread는 계속 실행됩니다. 이러한 설계는 Thread 생성 및 관리의 많은 복잡성을 숨깁니다.  
&nbsp;새로운 goroutine에서 함수 호출을 실행하려면 함수 앞에 **go** 키워드를 붙여 호출하면 됩니다. 호출한 함수가 완료되면 goroutine이 조용히 종료됩니다. Unix Shell에서 백그라운드에서 명령을 실행하는 **&** 와 유사합니다.  
&nbsp;함수가 참조하는 변수를 활성 상태로 지속되도록 보장하는 Function literal(Closure)은 goroutine에서 유용하게 사용됩니다.

~~~go
func main() {
    var waitGroup sync.WaitGroup

    waitGroup.Add(1)
	go GoRoutineTest(&waitGroup)

	// 모두 종료될때까지 대기
	waitGroup.Wait()
}

func GoRoutineTest(waitGroup *sync.WaitGroup) {
	defer waitGroup.Done()
	/* Body */
}}
~~~  

###### 2. Channels  
&nbsp;채널은 make를 사용하여 할당하며 결과값은 기본 데이터 구조에 대한 참조 역할을 하고 선택적으로 제공된 정수 매개변수는 채널의 Buffer 크기를 설정합니다. Buffer가 없거나 동기화된 채널의 기본 값은 0입니다. Buffer가 없는 채널은 통신(값 교환)과 동기화를 결합하여 두 개의 계산(goroutine)이 알려진 상태에 있음을 보장합니다.  
&nbsp;Receiver는 수신할 데이터가 있을 때까지 항상 차단합니다. Buffer가 없는 채널에서 Sender는 Receiver가 값을 수신할 때까지 차단됩니다. Buffer가 있는 채널에서 Sender는 Buffer에서 값이 복사 될 때까지만 차단됩니다. Buffer가 가득 찬 경우 일부 Receiver는 값을 반환할 때까지 기다립니다.  
&nbsp;Golang에서 가장 중요한 속성 중 하나로 최고 수준의 값인 채널이 다른 값과 동일하게 할당되고 전달될 수 있는 데, 이러한 속성을 안전한 병렬 다중화(Parallel demultiplexing) 구현을 위해 일반적으로 사용합니다.  
<br>
###### 3. 병렬화(Parallelization)
&nbsp;다중 CPU core에서 연산을 병렬화할 수 있습니다. 독립적으로 실행할 수 있는 연산의 일부분을 나눌 수 있는 경우 완료될 때 채널을 사용하여 병렬화할 수 있습니다. Concurrency(동시성, 독립적으로 실행되는 Component로 구성되는 프로그램)과 Parallelism(병렬화, 다중 CPU에서 효율성을 위해 병렬로 연산을 실행)을 혼동해서는 안됩니다. Golang의 Concurrency 기능으로 인해 일부 문제가 병렬 연산으로 쉽게 구성할 수 있지만, Golang은 Parallel이 아닌 Concurrent language이고 모든 병렬화 문제가 Golang의 모델에 맞지는 않습니다.  

&nbsp;아래는 병렬화시 사용되는 CPU관련 함수입니다.  
- **func NumCPU () int**
runtime package에 포함되어 있습니다. 현재 프로세스에서 사용 가능한 논리적 CPU의 수를 반환합니다.  

- **func GOMAXPROCS (n int ) int**
runtime package에 포함되어 있습니다. 동시에 실행할 수 있는 최대 CPU 수를 설정하고 이전 CPU 수는 반환합니다. 매개변수 n이 1보다 작으면 현재 설정은 변경되지 않습니다. 

###### [참고]  
- [병렬 프로그래밍](https://golang.org/doc/effective_go.html#concurrency)  
- [runtime 패키지](https://golang.org/pkg/runtime)