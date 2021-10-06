---
title: "Javascript Array Method"
date: "2020-09-05"
draft: false
path: "/blog/post-9"
---

<img src="https://media.vlpt.us/images/choichoikule/post/b48c9855-07e5-4ae8-a9c9-7db2262922d7/giphy.webp">

1일 1알고리즘을 풀다보니 내 지식에 한계를 느낀다.원래도 아는것은 별로 없었지만..아무튼 알고리즘을 풀 수 있는 메소드들의 사용법을 잘 인지하고 있어야 하는데 그렇지 못해서 돌아돌아 푸는 경우가 있다고 느꼈다. 해답을 봐도 메소드의 사용법부터 찾아봐야한다던지. 그래서 오늘부터 차근차근 자바스크립트 문법을 다시금 공부해보고자 한다. 먼저 가장 많이 쓰이는 Array에 대해서 조금씩 정리해보려고 한다.

---

# 표준 내장 객체 Array

Array는 자바스크립트의 표준 내장객체 중 하나이다. 표준 내장 객체(Standard Built-in Object)는 자바스크립트가 기본적으로 가지고 있는 객체들을 의미한다. 즉 내가 선언하지 않아도 사용할 수 있다는 말이다. 내장 객체가 중요한 이유는 프로그래밍을 하는데 기본적으로 필요한 도구들이기 때문으로 결국 프로그래밍이라는 것은 언어와 호스트 환경에 제공하는 기능들을 통해서 새로운 소프트웨어를 만들어내는 것이기 때문에 내장 객체에 대한 이해는 프로그래밍의 기본이라고 할 수 있다.

---

# Array의 property

property는 객체의 특징 중 하나이다. property는 보통 데이터 구조와 연관된 속성을 나타내고 property에는 2가지 종류가 있다.

- 인스턴스 property들은 특정 객체 인스턴스의 특정한 데이터를 가지고 있다.
- Static Property들은 모든 객체 인스턴스들에게 공유 된 데이터를 가지고 있다.

Array는 length 속성을 가지고 있는데 배열의 길이를 반환하고 양의 정수값과 2^32 미만의 값을 가질 수 있다. 여기서 배열의 길이라고 하면 0부터 시작하는 인덱스 끝자리 수가 반환되는 것으로 착각할 수도 있는데, 사실 배열의 길이보다는 배열에 들어있는 요소의 개수를 반환한다고 하는게 더 옳은 설명일 수 있겠다. 배열의 최대 인덱스보다 항상 크다. 아래 예시를 보면 쉽게 이해된다.

```javascript
> const clothing = ['shoes', 'shirts', 'socks', 'sweaters'];
> console.log(clothing.length);
>  // expected output: 4
> console.log(clothing.indexOf('sweaters'));
>  // expected output: 3
```

사실 @@unscopables속성도 가지고 있는데 with랑 같이 쓰이는 이 속성의 경우 with는 있어도 없는듯 살라는 명령어 중 하나이므로 이해를 과감히 포기하겠다ㅋㅋ

---

# Array의 method

두둥! 이 글을 작성하게 된 이유이다. 프로퍼티는 객체를 위해서 데이터를 저장한다면 메소드는 object가 요청 받을 수 있는 액션이다. 이 이상의 설명은 없을 듯 하다. 바로 method의 세계로 떠나보자.

<img src="https://media.giphy.com/media/d8i1XJjV2Ym53KK0Dn/giphy.gif" style="width:880px">

---

## Array.from()

유사 배열 객체(array-like object)나 반복 가능한 객체(iterable object)를 얕게 복사해새로운Array 객체를 만든다. 유사 배열 객체는 length 속성과 인덱싱된 요소를 가진 객체를 의미하고, 순회 가능한 객체 Map, Set 등객체의 요소를 얻을 수 있는 객체를 의미한다.

```javascript
> Array.from(arrayLike[, mapFn[, thisArg]])
```

- arrayLike: 배열로 변환하고자 하는유사 배열 객체나 반복 가능한 객체.
- mapFnOptional: 배열의 모든 요소에 대해 호출할 맵핑 함수.
- thisArgOptional: mapFn 실행 시에 this로 사용할 값.
- 반환 값: 새로운 Array 인스턴스.

```javascript
> Array.from('foo');
>  // ["f", "o", "o"]
> const mapper = new Map([['1', 'a'], ['2', 'b']]);
> Array.from(mapper.values());
>  // ['a', 'b'];
> Array.from([1, 2, 3], x => x + x);
>  // [2, 4, 6]
> Array.from({length: 5}, (v, i) => i);
>  // [0, 1, 2, 3, 4]
> const range = (start, stop, step) =>
> Array.from({ length: (stop - start) / step + 1}, (\_, i) => start + (i \* step));
> range(0, 4, 1);
>  // [0, 1, 2, 3, 4]
```

---

## Array.isArray()

인자가 Array인지 판별.

```javascript
> Array.isArray(obj)
```

- 반환값: 객체가 Array라면 true, 아니라면 false.

---

## Array.of()

인자의 수나 유형에 관계없이 가변 인자를 갖는 새 Array 인스턴스를 생성.
Array.of()와 Array 생성자의 차이는 정수형 인자의 처리 방법에 있다. Array.of(7)은 하나의 요소 7을 가진 배열을 생성하지만 Array(7)은 length 속성이 7인 빈 배열을 생성한다.

```javascript
> Array.of(7);
>  // [7]
> Array.of(1, 2, 3);
>  // [1, 2, 3]
> Array(7);
>  // [ , , , , , , ]
> Array(1, 2, 3);
>  // [1, 2, 3]
```

---

## Array.prototype.concat()

인자로 주어진 배열이나 값들을 기존 배열에 합쳐서 새 배열을 반환

```javascript
> array.concat([value1[, value2[, ...[, valueN]]]])
```

- 매개변수:배열 또는 값, 만약 value1 ~ valueN 인자를 생략하면 기존배열의 얕은 복사본을 반환.
- 반환값: 새로운 Array 객체

```javascript
> const array1 = ['a', 'b', 'c'];
> const array2 = ['d', 'e', 'f'];
> const array3 = array1.concat(array2);
> console.log(array3);
>  // expected output: Array ["a", "b", "c", "d", "e", "f"]
> console.log(array1.concat(1, [2, 3]));
>  // ['a', 'b', 'c', 1, 2, 3]
```

---

## Array.prototype.copyWithin()

배열의 일부를 얕게 복사한 뒤, 동일한 배열의 다른 위치에 덮어쓰고 그 배열을 반환합니다. 이 때, 크기(배열의 길이)를 수정하지 않고 반환

```javascript
> arr.copyWithin(target[, start[, end]])
```

- target: 복사한 시퀀스(값)를 넣을 위치를 가리키는 0 기반 인덱스. 음수를 지정하면 인덱스를 배열의 끝에서부터 계산.target이 arr.length보다 크거나 같으면 아무것도 복사하지 않음. target이 start 이후라면 복사한 시퀀스를 arr.length에 맞춰 자름.
- start Optional: 복사를 시작할 위치를 가리키는 0 기반 인덱스. 음수를 지정하면 인덱스를 배열의 끝에서부터 계산.기본값은 0으로, start를 지정하지 않으면 배열의 처음부터 복사.
- end Optional: 복사를 끝낼 위치를 가리키는 0 기반 인덱스. copyWithin은 end 인덱스 이전까지 복사하므로 end 인덱스가 가리키는 요소는 제외함. 음수를 지정하면 인덱스를 배열의 끝에서부터 계산.기본값은 arr.length로, end를 지정하지 않으면 배열의 끝까지 복사함.
- 반환 값: 수정한 배열.

```javascript
> [1, 2, 3, 4, 5].copyWithin(0, 2, 4);
>  // [3, 4, 3, 4, 5]
> [1, 2, 3, 4, 5].copyWithin(-2, -3, -1);
>  // [1, 2, 3, 3, 4]
```

---

## Array.prototype.entries()

배열의 각 인덱스에 대한 키/값 쌍을 가지는 새로운 Array Iterator 객체를 반환

```javascript
> arr.entries()
```

- 반환 값: Array 반복자 인스턴스 객체

자바스크립트에서 반복자(Iterator)는 시퀀스를 정의하고 종료시의 반환값을 잠재적으로 정의하는 객체이다. 더 구체적으로 말하자면, 반복자는 두 개의 속성(value, done)을 반환하는 next() 메소드를 사용하여 객체의 Iterator protocol을 구현한다. 시퀀스의 마지막 값이 이미 산출되었다면 done 값은 true 가 된다. 만약 value값이 done 과 함께 존재한다면, 그것은 반복자의 반환값이 된다.

반복자를 생성하면 next() 메소드를 반복적으로 호출하여 명시적으로 반복시킬 수 있다. 반복자를 반복시키는 것은 일반적으로 한 번씩만 할 수 있기 때문에, 반복자를 소모시키는 것이라고 할 수 있다. 마지막 값을 산출하고나서 next()를 추가적으로 호출하면 {done: true}. 가 반환된다.

```javascript
> var a = ['a', 'b', 'c'];
> var iterator = a.entries();
> for (let e of iterator) {
> console.log(e);
> }
> // [0, 'a']
> // [1, 'b']
> // [2, 'c']

> var sites = ['siteA','siteB','siteC'];
> sites_iterator = sites.entries();
> //Array Iterator {}
> console.log(sites_iterator.next())
> console.log(sites_iterator.next())
> console.log(sites_iterator.next())
> console.log(sites_iterator.next())
> //{value: [ 0, "siteA" ], done: false}
> //{value: [ 1, "siteB" ], done: false}
> //{value: [ 2, "siteC" ], done: false}
> //{value: undefined, done: true}
```

---

## Array.prototype.every()

배열 안의 모든 요소가 주어진 판별 함수를 통과하는지 테스트.
every는 callback이 거짓을 반환하는 요소를 찾을 때까지 배열에 있는 각 요소에 대해 한 번씩 callback 함수를 실행한다. 해당하는 요소를 발견한 경우 every는 즉시 false를 반환한다. 그렇지 않으면, 즉 모든 값에서 참을 반환하면 true를 반환한다. 할당한 값이 있는 배열의 인덱스에서만 callback을 호출한다. 삭제했거나 값을 할당한 적이 없는 인덱스에서는 호출하지 않는다.

```javascript
> arr.every(callback[, thisArg])
```

- callback: 각 요소를 시험할 함수. 다음 세 가지 인수를 받음.
- currentValue:처리할 현재 요소.
- index Optional: 처리할 현재 요소의 인덱스.
- array Optional: every를 호출한 배열.
- thisArg Optional: callback을 실행할 때 this로 사용하는 값.
- 반환 값: callback이 모든 배열 요소에 대해 참(truthy)인 값을 반환하는 경우 true, 그 외엔 false.

```javascript
> function isBigEnough(element, index, array) {
> return element >= 10;
> }
> [12, 5, 8, 130, 44].every(isBigEnough); // false
> [12, 54, 18, 130, 44].every(isBigEnough);// true
```

<br>
<br>
<br>
---

참고:
생활코딩  
MDN  
[property란무엇인가](https://m.blog.naver.com/magnking/220966405605)  
[배열메소드 entries알아보기](<https://webisfree.com/2020-04-27/[%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8]-%EB%B0%B0%EC%97%B4-%EB%A9%94%EC%86%8C%EB%93%9C-entries()-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0>)
