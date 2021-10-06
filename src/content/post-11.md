---
title: "Javascript Number Type"
date: "2021-12-30"
draft: false
path: "/blog/post-11"
---

<br>
<br>

## 자바스크립트의 number형

자바스크립트는 느슨한 타입 (loosely typed) 언어, 혹은 동적 (dynamic) 언어이다.  
그 말은, 변수의 타입을 미리 선언할 필요가 없다는 뜻이다.
타입은 프로그램이 처리되는 과정에서 자동으로 파악될 것이다.
또한 그 말은 같은 변수에 여러 타입의 값을 넣을 수 있다는 뜻이다.

ECMAScript 표준에 따르면, 숫자의 자료형은 배정밀도 64비트 형식 IEEE 754 값 (-(2^53 -1) 와 2^53 -1 사이의 숫자값) 단 하나만 존재한다. (IEEE 754는 IEEE에서 개발한 컴퓨터에서 부동소수점을 표현하는 가장 널리 쓰이는 표준이다. ±0 등의 수와 무한, NaN 등의 기호를 표시하는 법과 이러한 수에 대한 연산을 정의하고 있다.)

정수만을 표현하기 위한 특별한 자료형은 없다.  
부동 소수점을 표현할 수 있는 것 말고도, Number 타입은 세 가지 의미있는 몇가지 상징적인 값들도 표현할 수 있다. 이 값에는 +무한대, -무한대, and NaN (숫자가 아님)이 있다.
+/-Infinity 보다 크거나 작은지 확인하는 용도로 상수값인 Number.MAX_VALUE 나 Number.MIN_VALUE 을 사용할 수 있다.

또한, ECMAScript 6 부터는 숫자가 배정밀도 부동소수점 숫자인지 확인하는 용도로 Number.isSafeInteger() 과 Number.MAX_SAFE_INTEGER, Number.MIN_SAFE_INTEGER 을 사용할 수 있다. 이 범위를 넘어서면, 자바스크립트의 숫자는 더 이상 안전하지 않다.

Number 타입의 값 중에는 두 가지 방식으로 표현할 수 있는 유일한 값이 있는데, 0 이다. 0 은 -0 이나 +0 으로 표시할 수 있다. ("0" 은 물론 +0 이다.) 실제로는 이러한 사실은 거의 효력이 없다. 그 예로, +0 === -0 은 true 이다.

<br>

## 부동소수점

부동소수점 방식은 실수를 컴퓨터상에서 근사하여 표현할 때 소수점의 위치를 고정하지 않고 그 위치를 나타내는 수를 따로 적는 것으로, 유효숫자를 나타내는 가수와 소수점의 위치를 풀이하는 지수로 나누어 표현한다. 컴퓨터에서는 고정 소수점 방식보다 넓은 범위의 수를 나타낼 수 있어 과학기술 계산에 많이 이용된다.

부동 소수점에는 단정도와 배정도라는 두가지 표현방식이 있다. 단정도는 실수를 32비트로 표현하며 가장 왼쪽의 1 비트를 부호로 지수부를 8비트로 가수부를 23비트로 구성한다.
배정도는 실수를 64비트로 표현하며 부호 1비트, 지수부 11비트, 가수부 52비트로 구성된다. 실수를 표현하는 데 사용하는 비트 수가 단정도보다 두 배 많은 만큼 정밀도가 높게된다.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/88/General_floating_point_ko.svg/1000px-General_floating_point_ko.svg.png">

<br>
<br>

#### −118.625 (십진법)을 IEEE 754 (32비트 단정밀도)로 표현해 보자.

음수이므로, 부호부는 1이 된다.  
그 다음, 절댓값을 이진법으로 나타내면 1110110.101이 된다. (이진기수법을 참조)  
이진법으로 변환하는 방법은 일반정수의 경우 2로 나누어서 생기는 나머지를 기록해서 나눈 결과값이 1이 될때 종료하고, 소수점의 경우 곱하기 2를 하면서 소수점이 완전히 제거되면 종료하는 식이다.

소수점을 왼쪽으로 이동시켜, 왼쪽에는 1만 남게 만든다. 예를 들면 1110110.101=1.110110101×2⁶ 과 같다. 이것을 정규화된 부동소수점 수라고 한다.
가수부는 소수점의 오른쪽 부분으로, 부족한 비트 수 부분만큼 0으로 채워 23비트로 만든다. 결과는 11011010100000000000000이 된다.  
지수는 6이므로, Bias를 더해야 한다. 32비트 IEEE 754 형식에서는 Bias는 127이므로 6+127 = 133이 된다. 이진법으로 변환하면 10000101이 된다.
이 결과를 정리해서 표시하면 다음과 같다.
<br>
<br>

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Float_point_example_frac.svg/1200px-Float_point_example_frac.svg.png">

<br>
<br>
<br>
<br>
<br>

## 자바스크립트의 계산 오류

그런데 이렇게 10진수를 2진수로 변환하다보면 정확하게 변환되기 어려운 숫자들이 존재한다.
대표적인 것이 바로 0.1이다. 그래서 0.1 더하기 0.2 라는 계산을 입력하면 다음과 같은 이상한 계산을 내놓는다.

```javascript
const a = 0.1
const b = 0.2

console.log(a + b) //0.30000000000000004
```

<br>
<br>

제대로 계산하기 위해서는 toFixed()메서드나 Math객체, 기타 수학라이브러리를 활용하면 된다.  
Math는 수학적인 상수와 함수를 위한 속성과 메서드를 가진 내장 객체로 함수 객체가 아니며, Math는 Number 자료형만 지원하며 BigInt와는 사용할 수 없다.

### Number.prototype.toFixed()

toFixed() 메서드는 숫자를 고정 소수점 표기법으로 표기해 반환한다.

```javascript
function financial(x) {
  return Number.parseFloat(x).toFixed(2)
}

console.log(financial(123.456))
// expected output: "123.46"

console.log(financial(0.004))
// expected output: "0.00"

console.log(financial("1.23e+5"))
// expected output: "123000.00"

var numObj = 12345.6789

numObj.toFixed() // Returns '12346': 반올림하며, 소수 부분을 남기지 않습니다.
numObj.toFixed(1) // Returns '12345.7': 반올림합니다.
numObj.toFixed(6) // Returns '12345.678900': 빈 공간을 0으로 채웁니다.
;(1.23e20).toFixed(2) // Returns '123000000000000000000.00'
;(1.23e-10).toFixed(2) // Returns '0.00'
;(2.34).toFixed(1) // Returns '2.3'
;(2.35).toFixed(1) // Returns '2.4'. 이 경우에는 올림을 합니다.
;-(2.34).toFixed(1) // Returns -2.3 (연산자의 적용이 우선이기 때문에, 음수의 경우 문자열로 반환하지 않습니다...)
;(-2.34).toFixed(1) // Returns '-2.3' (...괄호를 사용할 경우 문자열을 반환합니다.)
```

<br>
<br>
<br>

참고:  
MDN  
[컴퓨터사이언스 부트캠프 with 파이썬](https://thebook.io/006950/ch03/03/)  
[위키피디아](https://ko.wikipedia.org/wiki/%EB%B6%80%EB%8F%99%EC%86%8C%EC%88%98%EC%A0%90)
[10진수-2진수(hexadecimal-binary)](https://box0830.tistory.com/155)
[자바스크립트 소수점 계산 오류 가볍게 이해하기](https://bigtop.tistory.com/47)
