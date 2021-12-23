---
title: "변수와 클로저와 스코프"
date: "2021-12-23"
draft: false
path: "/blog/variable"
---

var과 let, const의 차이는 누구나 기본적으로 잘알고 있을것이다.
그치만 이론적으로 아는 것과 상세히 동작과정을 살펴보는 것을 또 다르다.

## 스코프 범위

대부분의 프로그래밍언어는 블록레벨 스코프를 따르지만, 자바스크립트는 함수레벨 스코프를 따른다.
변수 호이스팅이 일어나서 선언전에 참조할 수 있는 것도 이 때문이다.
이것은 많은 문제를 야기할 수 있으므로 let과 const가 ES6부터 도입되었다.
let과 const는 var과 다르게 블록 레벨 스코프를 갖는다.

### var

```javascript
var foo = 123 // 전역 변수
console.log(foo) // 123
{
  var foo = 456 // 전역 변수
}
console.log(foo) // 456
```

### let

```javascript
let foo = 123 // 전역 변수
{
  let foo = 456 // 지역 변수
  let bar = 456 // 지역 변수
}
console.log(foo) // 123
console.log(bar) // ReferenceError: bar is not defined
```

let도 const도 호이스팅이 일어난다.
var과 다른점은 선언과 할당이 분리되어있다는 점이다.
할당 전까지 그 변수는 없는거나 마찬가지인상태다.

```javascript
let foo = 1 // 전역 변수
{
  console.log(foo) // ReferenceError: foo is not defined
  let foo = 2 // 지역 변수
}
```

## 반복문동작

### var

```javascript
for (var i = 0; i < 5; i++) {
  setTimeout(() => {
    console.log(i)
  }, 1000)
} // 5만 5번 출력된다.
```

for루프의 i가 var로 선언되어 전역변수로 등록된다.
그 이후에 태스크큐에 등록되어있던 셋타임아웃의 콜백이 나와서 이 전역변수를 출력한다.
for문을 돌면서 이 i는 계속 변하고 5까지 증가한다. for문이 돌면서 각각의 실행컨텍스트가 실행되지만
참조하고 있는 스코프는 전역의 i이다.

그렇다면 let으로 선언하면 어떻게 될까

### let

```javascript
for (let i = 0; i < 5; i++) {
  setTimeout(() => {
    console.log(i)
  }, 1000 * i)
}
```

이때엔 0부터 4까지 잘 출력된다.
스택이 비워져 큐의 콜백함수를 실행하는 시점에서의 i는 각 함수마다 생성되었던 scope의 i를 참조하게 되기 때문이다.

### 즉시실행 클로저

```javascript
for (var i = 0; i < 5; i++) {
  ;((i) => setTimeout(() => console.log(i), 1000 * i))(i)
}
```

이렇게 사용해도된다. 역시 클로저를 이용했다.

참고:  
[let과 const와 블록레벨 스코프](https://poiemaweb.com/es6-block-scope)
[반복문에서의 비동기함수](https://velog.io/@skawnkk/%EB%B0%98%EB%B3%B5%EB%AC%B8%EC%97%90%EC%84%9C%EC%9D%98-%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%95%A8%EC%88%98-%EC%8B%A4%ED%96%89)
