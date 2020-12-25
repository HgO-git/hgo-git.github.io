---
layoout: post
title:  "Golang 함수"
crawlertitle: post_golang_function
date:   2020-12-25 10:03:30 +0900
categories: golang
summary: "Golang에서 함수를 사용하는 방법에 대한 설명입니다."
---  
###### 1. Function 선언  
&nbsp;&nbsp;**func \<method_name\> ( _\<parameters\>_ ) _\<result_types\>_ { ... }**  
&nbsp;선언시 Function 이름을 Function의 식별자로 Binding합니다. 결과값을 선언하는 경우 함수 본문은 종료문(return)으로 끝내야합니다. Go 외부에서 구현된 Function에 대해 함수 본문 선언을 생략할 수도 있습니다.  

~~~go
func printToLower(str string) {
	fmt.Println(strings.ToLower(str))
}

func addInteger(left, right int) int {
	return (left + right)
}
~~~  

###### 2. Method 선언  
&nbsp;&nbsp;**func (\<receiver_name\> \<receiver_type\>) \<method_name\> ( _\<parameters\>_ ) _\<result_types\>_ { ... }**  
&nbsp;객체에 접근할 수 있는 Receiver가 첫 번째 인수로 있는 Function입니다. 선언시 Method 이름을 Method의 식별자로 Binding하며 Method를 Receiver의 기본 유형과 연결합니다.  
&nbsp;Receiver는 Method 이름 앞에 있는 추가 매개 변수 부분을 통해 지정됩니다. Receiver Parameter Section에는 변수가 아닌 단일 매개변수(Single Non-variadic Parameter)인 Receiver를 선언해야 합니다. Receiver의 기본 Type을 T라고 할때 Receiver의 Type은 정의된 Type(T) 또는 정의된 Type의 Pointer(*T)여야 합니다. Receiver의 기본 Type은 Pointer나 Interface Type이 될 수 없으며 Method와 동일한 Package에 정의되어 있어야 합니다. Method는 Receiver 기본 Type에 Binding되어 있으며 Method 이름은 T나 *T의 Selectors에서만 볼 수 있습니다.  
&nbsp;Function이나 Method의 Parameter와 동일하게 선언된 Receiver의 이름은 Method 안에서 고유해야 하며, Method 안에서 참조되지 않는 경우 선언이 누락됩니다. Receiver의 기본 Type이 struct인 경우 빈 값이 아닌 Method 이름과 Field 이름은 구별되어야 합니다.  

~~~go
// Receiver : p * Point
func (p * Point) Length () float64 { 
	return math.Sqrt (px * px + py * py) 
} 

func (p * Point) Scale (factor float64) { 
	px * = factor 
	py * = factor 
}
~~~

###### 3. 익명 함수(Anonymous Function, Lambda)  
&nbsp;참조를 위한 이름이 없는 함수입니다. 함수화는 필요하지만 함수가 많아질 수록 함수의 선언이 프로그램 전역으로 초기화되면서 메모리를 차지하여 프로그램 속도 저하를 야기합니다.  

~~~go
type integerFunc func(int, int) int

func main() {
	var intFunc integerFunc

	// 덧셈
	intFunc = func(left, right int) int { return left + right }
	fmt.Printf("5+4 = %d\n", intFunc(1, 4))

	// 뺄셈
	intFunc = func(left, right int) int { return left - right }
	fmt.Printf("5-4 = %d\n", intFunc(1, 4))

	// 곱셈
	intFunc = func(left, right int) int { return left * right }
	fmt.Printf("5*4 = %d\n", intFunc(1, 4))

	// 나눗셈
	intFunc = func(left, right int) int { return left / right }
	fmt.Printf("5/4 = %d\n", intFunc(1, 4))
}
~~~  

###### 4. Closure  
&nbsp;Golang에서 익명 함수를 함수 밖에 있는 변수를 참조하는 Closure로 사용할 수 있습니다.  
&nbsp;아래는 익명 함수 ``func(damage int) int``를 반환하는 monsterHealth에 대한 예입니다. remainHealth는 monsterHealth 함수 호출시 초기화되어 반환한 익명 함수를 호출할 때마다 damage 계산을 통해 감소시킵니다.  
~~~go
func main() {
	redSlime := monsterHealth(1, 25400)

	fmt.Printf("Attack 100 -> %d\n", redSlime(100))
	fmt.Printf("Attack 70 -> %d\n", redSlime(70))
	fmt.Printf("Attack 140 -> %d\n", redSlime(140))
	fmt.Printf("Attack 95 -> %d\n", redSlime(95))
}

func monsterHealth(monsterType byte, health int) func(damage int) int {
	remainHealth := health

	switch monsterType {
	case 1: // 슬라임(Level 2~10) : 회피로 피해량 10% 감소
		return func(damage int) int {
			remainHealth -= (int)((float32)(damage) * 0.1)
			return remainHealth
		}
	case 2: // 두 발로 걷는 돼지(Level 11~15) : 피해량 20% 감소
		return func(damage int) int {
			remainHealth -= (int)((float32)(damage) * 0.2)
			return remainHealth
		}
	}

	return func(damage int) int {
		remainHealth -= damage
		return remainHealth
	}
}
~~~  

- 실행 결과 : 
~~~text
Attack 100 -> 25390
Attack 70 -> 25383
Attack 140 -> 25369
Attack 95 -> 25360
~~~

###### [참고]
- [Golang의 함수 설명](https://golang.org/ref/spec#Function_declarations)  
