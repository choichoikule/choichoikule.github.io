---
title: "Nestjs을 시작하기전에 - 데코레이터"
date: "2021-09-07"
draft: false
path: "/blog/post-2"
---


> " Nest는 데코레이터라는 언어기능을 중심으로 구축되었습니다.
> ES2016 데코레이터는 함수를 반환하고 대상, 이름 및 속성 설명자를 인수로 사용할 수 있는 표현식입니다. 
> 데코레이터 앞에 @문자를 붙이고 이를 데코하려는 항목의 맨 위에 배치하여 적용합니다. 데코레이터는 클래스, 메서드 또는 속성에 대해 정의할 수 있습니다. "

NestJS는 NodeJS 서버 어플리케이션의 아키텍쳐의 문제점을 해결하고자 만들어진 프레임워크이다. 
NestJS는 테스트하기 쉽고 확장가능한 아키텍쳐를 제공하며, 우리는 NestJS가 정의한 데코레이터를 사용하여
손쉽게 이러한 아키텍쳐 기반위에 웹 서버를 구축할 수 있다.

데코레이터는 전달된 함수 또는 메서드의 동작을 수정하여 새 함수를 반환하는 함수이다.
자바스크립트는 객체의 속성에대해서 값과 함께 플래그라고 불리는 구성가능,열거가능,쓰기가능을 추적하고 관리한다. 
각각은 boolean으로 관리되며 평범한 방식으로 프로퍼티를 생성한 경우 모두 true이다.
데이터속성이외에 접근자속성을 가지는데 getter, setter가 바로 그것이다.

데코레이터(Decorator)는 ECMAScript에 새롭게 제안된 기능이며, TypeScript의 실험적 기능으로 클래스 선언과 멤버에 대한 주석(annotations)과 메타 프로그래밍 구문을 모두 추가 할 수있는 방법을 제공합니다.
데코레이터 사용 설정
데코레이터를 TypeScript에서 사용하려면 tsconfig.json 설정 experimentalDecorators 값을 true로 설정해야 합니다. 환경 설정 없이 데코레이터를 사용하려면 컴파일 과정에서 오류 메시지를 출력합니다.
NestJS tsconfig.json을 봐도 해당 설정이 되어있는 것을 확인할 수 이따.
데코레이터는 클래스, 속성, 메서드, 접근 제어자, 매개변수 등에 사용할 수 있는 특별한 함수입니다. 선언된 데코레이터 함수를 사용할 때는 데코레이터 이름 앞에 @를 붙입니다.
함수 사용 시, 사용자가 인자를 전달할 수 있는 것과 유사하게, 데코레이터 함수 또한 팩토리를 사용해 사용자로부터 인자를 전달 받도록 설정할 수 있습니다. 데코레이터 팩토리 함수는 데코레이터 함수를 감싸는 래퍼 함수입니다. 팩토리는 사용자로부터 전달 받은 인자를, 내부에서 반환되는 데코레이터 함수는 데코레이터로 사용됩니다. 

```javascript
// 데코레이터 팩토리
function Component(value:string) {
  console.log(value);
  
  // 데코레이터 함수
  return function(target:Function) {
    console.log(target);
    console.log(target.prototype);
  }
  
}

// 데코레이터 팩토리를 사용하면 값을 전달할 수 있습니다.
@Component('tabs')
class TabsComponent {}

// TabsComponent 객체 생성
const tabs = new TabsComponent();
```

참고
[타입스크립트 핸드북](https://yamoo9.gitbook.io/typescript/decorator/composition)