---
title: "Solidity-2"
date: "2022-01-27"
draft: false
path: "/blog/19"
---

컨트랙트는 객체지향언어에서의 클래스와 유사하다. 각 컨트랙트들은 상태변수, 함수. 함수 modifier, 이벤트, 에러, struct 타입, enum타입 등의 선언을 포함한다. 컨트랙트들은 상속이 가능하며 라이브러리와 인터페이스라고 하는 특별한 컨트랙트들이 존재한다.

## type

적절한 상태변수들을 정의하기 위해서 솔리디티에 존재하는 데이터 타입을 알아야한다.

1. bool
   (operator: !, &&, ||, ==, !=)
2. int 부호있는 정수 타입 / uint 부호없는 정수 타입
   (operator: <=, <, ==, !=, >=, >, &, |, ^, ~, <<, >>, +, -, \*, /, %, \*\*)
   int8 ~ int256, uint8 ~ uint256 범위 지정 가능, 256이 기본
   \*int의 operator에서 |,^,~는 비트연산자이다. 비트연산자를 사용하면 메모리 최적화를 할 수 있게 된다. int.min, int.max 와 같은 식으로 지정한 범위에서 최대최소값을 확인할 수 있다.
   x << y 는 x \* 2**y 와 동일하며, x >> y 는 x / 2**y 와 동일하다. 이는 음수를 시프트하는 경우 부호가 확장됨을 의미.음수만큼 시프트 연산을 실행하는 경우 런타임 예외가 발생한다. 부호있는 음의 정수를 우측시프트 연산한 결과값은 다른 프로그래밍 언어와 다르게 음의 값이 0으로 반올림되어진다.
3. fixed 다양한 크기의 부호있는 고정 소수점/ ufixed 부호없는 고정 소수점 타입
   ufixed 와 fixed 는 각각 ufixed128x19 와 fixed128x19 의 별칭임
   fixedMxN형식에서 M은 타입에 의해 취해진 비트의 수를, N은 소수점 이하 자리수를 나타낸다.
   고정 소수점은 아직 완벽하게 지원되지 않는다 선언은 되지만 할당은 되지 않는다. 일반적으로, 부동 소수점 방식에서는 거의 모든 공간이 소수 부분을 나타내기 위해 사용되지만 고정 소수점 방식에서는 적은 수의 비트만이 소수 부분을 정의하는데 사용된다.
   (operator: <=, <, ==, !=, >=, >, +, - , \*, /, %)
4. address 20바이트의 이더리움 address를 담을 수 있다.
   (operator: <=, <, ==, !=, >=, >)
   5.1. <address>.balance 잔고조회
   5.2. <address>.transfer(uint256 amount) 이더 전송
   5.3. <address>.send(uint256 amount) returns (bool)
   5.4. <address>.call(...) returns (bool) abi를 준수하지 않는 컨트랙트와 상호작용을 위함
   5.5. <address>.callcode(...) returns (bool)
   5.6. <address>.delegatecall(...) returns (bool)
5. byte, bytes1 ~ bytes32 고정크기 바이트 배열
   6.1. <byte>.length
6. bytes 동적크기 바이트 배열
7. string 동적 크기의 UTF-8 인코딩된 문자열
8. 유리수 리터럴, 정수 리터럴, address 리터럴
9. enum
10. hex 16진수 리터럴

함수

1. publicc 해당 컨트랙트 내에서만
2. internal 상속한 곳에서까지
3. public 내부외부 모두
4. external 외부에서만
5. view 어떤 데이터도 저장 변경하지 않음
6. pure 어떤 데이터도 읽지않고 어던 데이터도 저장 변경하지 않음
7. modifier
8. payable

참고:  
[비트단위연산자](https://boycoding.tistory.com/163)
