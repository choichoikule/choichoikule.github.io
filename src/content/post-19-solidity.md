---
title: "[번역과 공부 그 사이 어딘가]Solidity-Structure of a Contract"
date: "2022-01-26"
draft: false
path: "/blog/19"
---

컨트랙트는 객체지향언어에서의 클래스와 유사하다. 각 컨트랙트들은 상태변수, 함수. 함수 modifier, 이벤트, 에러, struct 타입, enum타입 등의 선언을 포함한다. 컨트랙트들은 상속이 가능하며 라이브러리와 인터페이스라고 하는 특별한 컨트랙트들이 존재한다.

## type
적절한 상태변수들을 정의하기 위해서 솔리디티에 존재하는 데이터 타입을 알아야한다. 
1. bool (operator: !, &&, ||, ==, !=)
2. int  (operator: <=, <, ==, !=, >=, >, &, |, ^, ~, <<, >>, +, -, *, /, %, **)
3. int8
4. int256
5. uint
6. uint8
7. uint256

*int의 operatordptj |,^,~는 비트연산자이다. 비트연산자를 사용하면 메모리 최적화를 할 수 있게 된다. 


참고:  
[비트단위연산자](https://boycoding.tistory.com/163)