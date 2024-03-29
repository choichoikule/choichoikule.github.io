---
title: "Core Javascript"
date: "2020-12-20"
draft: false
path: "/blog/post-10"
---

### 기본형과 참조형

자바스크립트의 데이터 타입에는 기본형과 참조형 두가지가 있다.
기본형 Primitive type에는 String, number, boolean, null, undefined, Symbol 6가지가 있고, 참조형 Reference type에는 Object가 있고, Array, Function,Date, RegExp, Map, Set등이 객체의 하위분류에 속해있다. 기본형은 데이터가 저장된 주소값을 저장하고, 참조형은 데이터가 저장된 주소값 묶음을 저장한다고 표현할 수 있다. 이 말을 좀더 이해하려면 위해 데이터가 메모리에 저장되는 과정을 봐야한다.

### 데이터가 저장될 때

```javascript
var a = 1
```

자바스크립트에서 위의 데이터 할당이 이루어지는 과정은 다음과 같다.

1. 빈공간 확보
2. 빈공간 이름 식별자 a로 지정
3. 다른 빈공간에 1 저장
4. 식별자 a에 1이 저장된 공간의 주소값 저장

```javascript
var a = 2
```

여기서 위와 같이 변수를 수정했다고 하면 다음과 같은 과정이 일어날 것이다.

1. 빈공간에 2 저장
2. 식별자 a에 2가 저장된 공간의 주소값으로 데이터 변경

```javascript
var c = { x: 3, y: 4 }
```

객체형 데이터가 할당될 때에는 조금 다르게 작동한다.

1. 빈공간 확보
2. 식별자 c로 빈공강 명명
3. 다른 빈공간 확보 @1
4. x명명
5. 다른 빈공간 확보 @2
6. y명명
7. 빈공간 확보후 3과 4 할당 후 @1,@2에 주소값 할당
8. 식별자c로 명명된 공간에 @1~@2저장
   여기서 x,y가 수정될 경우 @1~@2라는 주소묶음은 변하지 않고, @1과 @2에 저장되어 있는 주소값만 변한다.

### 데이터가 복사 후 변경될 때

```javascript
var a = 1 // 1이 @100에 저장
var b = a // a,b에 @100저장
console.log(a) // 1출력
console.log(b) // 1출력
b = 2 // 2 @101에 저장후 b에 @101저장
console.log(a) // 1출력
console.log(b) // 2출력

// a = @100 b = @101을 가리키고 있음

var obj = { x: 7, y: 8 }
// 빈공간 obj명명
// @102에 x명명, @103에 y명명
// @104에 7저장, @105에 8저장
// x에 @104저장, y에 @105저장
// @102~@103를 @105에 저장
// obj에 @105저장
var obj2 = obj //obj2라고 명명된 공간 @105저장
console.log(obj) // {x:7,y:8}출력
console.log(obj2) // {x:7,y:8}출력
obj2.x = 2 // @106에 2저장 후 x에 @106저장 x의 위치 @102는 불변
console.log(obj) // {x:2,y:8}출력
console.log(obj2) // {x:2,y:8}출력
obj2 = { x: 9, y: 10 } // 객체자체를 변경함
console.log(obj) // {x:2,y:8}출력
console.log(obj2) // {x:9,y:10}출력
```

좀더 엄밀히 말하면 x를 식별자로 지정할때 obj의 속성인 X라고 메모리 상에서 명명되는 것으로 생각하면 좋을 것 같다. obj2.x로 접근하는 경우 주소값을 복제해왔으므로 obj.x에 접근하는 것과 동일한 결과를 출력하지만 obj2 ={}이런식으로 객체를 아예 재 선언하는 경우는 obj2의 속성인 x라고 메모리 상에서 새롭게 명명되기 떄문에 이전 객체와는 달라지는 것이다.

```javascript
var obj3 = { x: 1, x: 2, x: 3 }
console.log(obj3) /// {x:3}출력
```

위와 같이 코드를 쳐봐도 알 수 있다. obj3의x는 하나이므로 x:3이 된다.
복사할때 나타나는 객체의 이러한 성질 때문에 객체를 복사해서 사용할 경우 불변객체가 필요한지 아닌지를 잘 판단해야하며, 불변객체가 필요할 경우는 객체의 내부프로퍼티들까지 for문으로 돌면서 하나하나 복사해주는 함수를 작성하던지, JSON객체로 바꿨다가 다시 가져오거나, immutable js와 같은 도구들을 사용해야한다.

### undefined와 null

undefiend 는 변수는 존재하나, 어떠한 값으로도 할당되지 않아 자료형이 정해지지(undefined) 않은 상태를 말하고 null은 변수는 존재하나, null 로 (값이) 할당된 상태. 즉 null은 자료형이 정해진(defined) 상태입니다. null의 typeof출력결과는 Object이다.

```javascript
var obj4 = {}
var obj5 = {}
console.log(obj4 === obj5) // false
console.log(obj4 == obj5)
// false 빈객체는 서로 다름 메모리에 할당되는 방식을 생각하면 이해된다.

var a = undefined
var b = undefined
console.log(a === b) // true

var c = null
var d = null
console.log(c === d) // true
console.log(a === c) // false
console.log(a == c) // true
console.log(a === obj4) // false
```

undefined는 메모리주소를 지정하지 않은 식별자에 **접근**하거나, 객체내부 존재하지않는 프로퍼티에 **접근**하거나, 리턴문이 없거나 호출되지 않는 함수의 실행결과를 **나타낼때** 자바스크립트 엔진이 자동으로 부여하게 된다. 처음부터 undefined를 할당하는것이아니라 접근할때 반환하는 것임을 알아둬야함. 비어있는 요소와 undefined는 엄연히 다른 상태이다. 처음 언급했듯이 undefined도 데이터 기본형 중 하나임.

```javascript
var arr = []
arr.length = 3
console.log(arr) //<3 empty items> 출력

var arr2 = [undefined, undefined, undefined]
console.log(arr2) //[ undefined, undefined, undefined ] 출력
```

배열도 객체이고, 배열에서 값이 지정되지 않은 인덱스는 아직 존재하지 않는 프로퍼티라고 생각하면 된다.
<br>
<br>

---

<br>

### 실행 컨텍스트

실행컨텍스트는 실행할 코드에 필요한 환경을 모아놓은 환경을 뜻하는 추상적인 개념으로 변수, 스코프, this 3가지로 구성되며 자바스크립트 엔진은 물리적인 객체 형태로 실행컨텍스트를 관리한다.

#### 콜 스택

콜 스택(call stack) 이란 컴퓨터 프로그램에서 현재 실행 중인 서브루틴에 관한 정보를 저장하는 스택 자료구조이다. 자바스크립트코드가 실행될 때 앞서 말한 실행컨텍스트가 콜스택에 쌓인다. 맨처음 파일을 열었을때 가장 먼저 실행되는 것은 전역 컨텍스트이고 그 이후부터 차례대로 실행되는 함수들의 실행 컨텍스트가 차례대로 쌓인다.

#### 변수

실행컨텍스트에서 변수정보를 수집할 때 변수의 선언부들을 가장 먼저 처리한다. 변수 선언부가 스코프의 맨 위로 끌어올려진 것 처럼 느껴지기 때문에 이를 호이스팅이라고도 한다. 할당은 위로 끌어올려지지 않는다. 다만 **함수선언의 경우엔 전체를 끌어올리므로 할당까지 마친것처럼 보인다.** 이를 피하고 싶을 경우 함수를 변수에 할당하는 함수 표현식을 사용하면 된다.

```javascript
function a() {
  console.log(b) // f b() 출력
  var b = "bbb"
  console.log(b) // 'bbb'출력
  function b() {}
  console.log(b) // 'bbb'출력
}
a()

console.log(sum(1, 2)) // 3출력
console.lgo(multiply(3, 4)) //multiply is not a function

function sum(a, b) {
  //함수선언
  return a + b
}

var multiply = (a, b) => {
  //함수표현
  return a * b
}
```

<br>

#### 스코프

스코프란 식별자에 대한 유효범위이며 내부에서 부터 훑어나가서 가장 먼저 발견된 식별자에만 접근 할 수 있으며, 외부에서 내부는 접근할 수 없다. 전역에서 선언되어 어디서나 접근 가능한 변수를 전역변수라고 하며 그렇지 않은 변수들을 지역 변수라고 한다.

```javascript
var a = 1
var outer = function () {
  var inner = function () {
    console.log(a)
    var a = 3
  }
  inner() // inner의 스코프에서 a는 선언되었지만 할당안됨
  //undefined 출력, inner는 실행 종료되고 콜스택에서 사라짐
  console.log(a)
}
outer()
// outer의 스코프에서는 a찾을 수 없음 그 밖에있는 외부에서 a찾음 1출력
console.log(a) // a=1출력
```

<br>

#### this

자바스크립트에서 this는 실행컨텍스트가 생성될때 함께 결정된다.전역공간에서의 this는 항상 전역객체를 가리킨다. 전역객체의 프로퍼티로 할당하는 것과 동일한 결과라고 생각하면 되나, 다만 전역변수로 선언했을때는 삭제가 되지않고 프로퍼티로 할당했을땐 삭제가 된다는 차이가 있다. 함수내부에서 this는 함수가 함수 그 자체로 호출되었을 때는 전역객체를, 함수가 어떠한 객체의 메소드로서 호출되었을때는 자신을 호출한 객체를 가리킨다. 만약 그냥 함수로 썼는데도 불구하고 전역객체가 아니라 자신이 호출된 환경을 this로 바인딩하고 싶다면 변수에 저장해서 전달하는 방식으로 사용할 수 있다. 화살표함수를 쓰는경우 이러한 변수저장필요없이 상위스코프를 this로 바로 사용할 수 있다. 또한 콜백함수에서는 this를 임의로 지정할 수 있으며 생성자함수내부에서의 this는 생성자함수를 통해 만들어질 인스턴스를 가리키게된다.

```javascript
var obj1 = {
  outer: function () {
    console.log(this)
    var innerfunc = function () {
      console.log(this)
    }
    innerfunc()
    var obj2 = {
      innerMethod: innerfunc,
    }
    obj2.innerMethod()
  },
}
obj1.outer()

// {outer:f}
// window
// {innerMethod:f}차례대로 출력
```

<br>

#### this를 내맘대로 call, apply, bind

call과 apply함수를 활용하면 this를 임의로 지정할 수 있다.
call과 apply함수는 인수를 전달받아 즉시 실행하는 함수이다.

```javascript
var obj={
  a=1,
  method: function(x,y){
    console.log(this.a,x,y);
  }
};
obj.method.apply({a:4},[5,6]); // 456출력

function Person(name,gender){
  this.name=name;
  this.gender=gender;
}

function Student (name,gender,school){
  Person.call(this,name,gender);
  this.school=school;
}

var by = new Student ('보영','female','단국대');
console.log(by);
// Student {
//  name: '보영',
//  gender: 'female',
//  school: '단국대',
//  __proto__: Student { constructor: ƒ Student() } 출력
}

```

<br>
<br>
<br>
<br>

참고: 코어자바스크립트  
https://siyoon210.tistory.com/148
