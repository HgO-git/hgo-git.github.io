---
layoout: post
title:  "Golang에서 string 사용"
crawlertitle: post_golang_use_string
date:   2021-01-16 23:14:39 +0900
categories: golang
summary: "Golang에서 string 타입에 대한 설명입니다."
---  
###### 1. 문자열 출력  
&nbsp;fmt 패키지를 사용하여 I/O를 구현할 수 있습니다. 이때, 사용되는 함수명은 C와 동일합니다.  

~~~go
func main() {
	str := fmt.Sprintf("Now : %s", time.Now().Format("2006.01.02 15:04:05"))
	fmt.Println(str)
}
~~~

###### 2. 비교  
&nbsp;**2.1. 같음**  
- **"\<string_left\>" == "\<string_right\>"**  
문자열 "\<string_left\>"와 "\<string_right\>"를 대소문자를 구분하여 비교합니다.  
- **func EqualFold(s, t string) bool**  
UTF-8 문자열 s와 t를 대소문자를 구분하지 않는 Unicode case-folding으로 비교합니다.  

~~~go
func main() {
	if "A" == "a" {
		fmt.Println("두 문자열이 같습니다.")
	} else {
		fmt.Println("두 문자열이 다릅니다")
   }
   
	if strings.EqualFold("B", "b") {
		fmt.Println("두 문자열이 같습니다.")
	} else {
		fmt.Println("두 문자열이 다릅니다")
	}
}
~~~

&nbsp;**2.2. 접두사, 접미사**  
- **func HasPrefix(s, prefix string) bool**  
문자열 s가 문자열 prefix로 시작하면 true, 그렇지 않으면 false입니다.  

- **func HasSuffix(s, suffix string) bool**  
문자열 s가 문자열 suffix로 끝나면 true, 그렇지 않으면 false입니다.  

~~~go
func main() {
   if strings.HasPrefix("The old man and the Sea", "The old man") {
		fmt.Println("'The old man'으로 시작합니다.")
	}
	if strings.HasSuffix("The old man and the Sea", "the Sea") {
		fmt.Println("'the Sea'으로 끝합니다.")
   }
}
~~~

&nbsp;**2.3. 포함**
- **func contains(s, substr string) bool**  
문자열 s에 문자열 substr이 포함되는 경우 true, 그렇지 않으면 false입니다.

~~~go
func main() {
	if strings.Contains("The old man and the Sea", "old man") {
		fmt.Println("'old man'이 포함됩니다.")
	}
}
~~~

###### 3. 가공    
&nbsp;**3.1. 구분 문자열에 따라 분할**  
- **func Split(s, sep string) [] string**  
문자열 s를 구분 문자열 sep로 분할하여 반환합니다. 만약 구문 문자열 sep가 포함되어 있지 않으면 분할이 불가능하므로 s를 그대로 반환합니다.   

~~~go
func StringSplitRemoveEmpty(source, separator string) []string {
	var result []string
	for _, str := range strings.Split(source, separator) {
		if str == "" {
			continue
		}
		result = append(result, str)
	}

	return result
}
~~~

&nbsp;**3.2. 특정 문자열 제거**  
- **func Trim(s, cutset string) string**  
문자열 s에 포함된 cutset을 모두 제거하여 반환합니다.  
- **func TrimPrefix(s, prefix string) string**  
문자열 s의 접두사 prefix를 제거하여 반환합니다.  
- **func TrimSuffix(s, suffix string) string**  
문자열 s의 접미사 suffix를 제거하여 반환합니다.  
- **func TrimSpace(s string) string**
문자열 s의 앞뒤에서 공백을 제거하여 반환합니다.  

~~~go
func main() {
	fmt.Println(strings.TrimSpace(" The old man and the Sea "))
}
~~~  

&nbsp;**3.3. 특정 문자열 대체**  
- **func Replace(s, old, new string, n int) string**  
문자열 s에 포함되는 old 문자열들 중 n개만 new 문자열로 대체하여 반환합니다. n 값이 0보다 작으면 문자열 s에 포함되는 모든 old 문자열을 대체합니다. old 문자열이 비어있으면 문자열 시작과 각 UTF-8 시퀀스 후부터 일치하여 k-rune 문자열에 대해 최대 k+1개를 대체합니다.   
- **func ReplaceAll(s, old, new string) string**  
문자열 s에 포함되는 모든 old 문자열을 new 문자열로 대체하여 반환합니다.  

~~~go
func main() {
	path := "work\\project_01\\info.txt"
	fmt.Println(strings.Replace(path, "\\", "/", -1))
}
~~~

###### 4. 변환  
&nbsp;**4.1. Byte Array**  
- **[]byte(\<string\>)**  
문자열 \<string\>를 Byte array로 변환합니다.  
- **string(\<byte_array\>)**  
Byte array를 문자열로 변환합니다.  

~~~go
func main() {
	bytes := []byte{112, 105, 103, 101, 111, 110}
	str := "The old man and the Sea"

	fmt.Println(string(bytes)) // byte array -> string
	fmt.Println([]byte(str))   	// string -> byte array
}
~~~  

&nbsp;**4.2. Int**  
- **func ParseInt(s string, base int, bitSize int) (i int64, err error)**  
주어진 base와 bitSize 값에 따라 문자열 s를 해당하는 int 값을 변환하여 반환합니다. base에는 0, 2 ~ 36 값을 사용할 수 있으며 int에 대한 진법을 의미합니다. bitSize는 실제로 반환될 값의 Type으로 0 ~ 64 값을 사용할 수 있으며 0, 8, 16, 32, 64인 경우 int, int8, int16, int32, int64가 됩니다. 반환되는 Error에는 정확한 Error 정보를 갖는 NumError를 포함하므로 Syntax Error인지 bitSize를 넘겼는지 알 수 있습니다.  

- **func Atoi(s string) (int, error)**  
문자열 s를 int로 변환합니다. ParseInt(s, 10, 0)과 동일합니다.  

- **func FormatInt(i int64, base int) string**  
int64인 i 값을 주어진 2 ~ 36 사이의 base 값에 따라 문자열로 변환하여 반환합니다. 결과는 10이상인 자릿수에 대해 소문자 a ~ z를 사용합니다.  

- **func Itoa(i int) string**  
int 값 i를 문자열로 변환합니다. FormatInt(int64(i), 10)과 동일합니다.  

~~~go
func main() {
	var intNum int
	var uint64Num uint64
	var strInt string
	var err error

	// int -> string
	strInt = strconv.Itoa(31)
	fmt.Printf("Int를 문자열로 변환 : %s\n", strInt)

	// string -> int
	if intNum, err = strconv.Atoi(strInt); err != nil {
		fmt.Printf("문자열을 Int로 변환 실패 : %v\n", err)
	} else {
		fmt.Printf("문자열을 Int로 변환 : %d\n", intNum)
	}

	// uint64 -> string
	strInt = strconv.FormatUint(40240, 10)
	fmt.Printf("UInt64를 문자열로 변환 : %s\n", strInt)

	// string -> uint64
	if uint64Num, err = strconv.ParseUint(strInt, 10, 32); err != nil {
		fmt.Printf("uint64로 변환 실패 : %v\n", err)
		if errEnum, valid := err.(*strconv.NumError); valid {
			switch errEnum.Err {
			case strconv.ErrRange:
				fmt.Printf("\tError Range\n")
			case strconv.ErrSyntax:
				fmt.Printf("\tError Syntax\n")
			}
		}
	} else {
		fmt.Printf("문자열을 Int로 변환 : %d\n", uint64Num)
	}
}
~~~

&nbsp;**4.3. Float**  
- **func ParseFloat(s string, bitSize int) (float64, error)**  
문자열 s를 bitSize 값으로 지정된 정밀도를 갖는 부동 소수점 숫자로 변환하여 반환합니다. bitSize가 64면 float64, 32이면 type은 float64이지만 float32로 변환이 가능합니다.  
10진수와 16진수 부동 소수점 구문을 허용하며 IEEE 754 unbiased rounding(banker's rounding)을 사용하여 반올림한 가장 가까운 부동 소수점을 반환합니다. 가수에 맞는 16진수 표현보다 더 많은 Bit가 있으면 16진수를 부동 소수점으로 Parsing할 때 반올림합니다.  
"NaN", "Inf"(또는 "Infinity") 문자열(대소문자 구분하지 않음)을 특수한 부동 소수점 값으로 인식합니다.  

- **func FormatFloat(f float64, fmt byte, prec, bitSize int) string**  
부동 소수점 숫자 f를 fmt 포맷과 정밀도 perc에 따라 문자열로 변환하여 반환합니다. 원래 값이 bitSize의 부동 소수점 값에서 획득했다고 가정하여 결과를 반올림합니다. ParseFloat와 동일하게 bitSize가 32면 float32, 64면 float64입니다. 정밀도 perc는 fmt 포맷의 실수부에 해당하는 숫자를 제어합니다. fmt가 'e', 'E', 'f', 'x', 'X'인 경우 소수점 이하 자릿수, 'g', 'G'인 경우 최대 유효 자릿수(맨 끝의 0은 제거)입니다. 특별하게 perc가 -1이면 f를 문자열로 변환하여 반환할 때 필요한 최소 자릿수를 사용합니다. 아래는 실수의 형식을 설정하는 fmt 값에 대한 설명입니다.  

	|fmt|Format|설명|
	|:---:|:----|:----|
	|b|-ddddp±ddd|이진 지수|
	|e|-d.dddde±dd|10진수 지수|
	|E|-d.ddddE±dd|10진수 지수|
	|f|-ddd.dddd|지수 없음|
	|g||지수 값이 크면 'e', 그렇지 않은 경우 'f'|
	|G||지수 값이 크면 'E', 그렇지 않은 경우 'f'|
	|x|-0xd.ddddp±ddd|16진수 분수와 이진 지수|
	|X|-0Xd.ddddP±ddd|16진수 분수와 이진 지수|

~~~go
func main() {
	var float32Num float32
	var strFloat string
	var err error

	// float32 -> string
	strFloat = strconv.FormatFloat(8.15, 'f', -1, 64)
	fmt.Printf("Float를 문자열로 변환 : %s\n", strFloat)

	// string -> float32
	if float32Num, err = strconv.ParseFloat(strFloat, 8); err != nil {
		fmt.Printf("float64로 변환 실패 : %v\n", err)
		if numError, valid := err.(*strconv.NumError); valid {
			switch numError.Err {
			case strconv.ErrRange:
				fmt.Printf("\tError Range\n")
				break
			case strconv.ErrSyntax:
				fmt.Printf("\tError Syntax\n")
				break
			}
		}
	} else {
		fmt.Printf("문자열을 Float로 변환 : %f\n", float32Num)
	}
}
~~~

###### [참고]  
- [fmt 패키지](https://golang.org/pkg/fmt)
- [strings 패키지](https://golang.org/pkg/strings)
- [strconv 패키지](https://golang.org/pkg/strconv)