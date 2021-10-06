---
title: "Nestjs를 시작하기전에 "
date: "2021-09-07"
draft: false
path: "/blog/post-2"
---

Nestjs로 서버를 구축하면서 공부가 필요했던 개념들을 정리해 보려고 한다.

## 데코레이터

NestJS는 NodeJS 서버 어플리케이션의 아키텍쳐의 문제점을 해결하고자 만들어진 프레임워크로 데코레이터라는 언어기능을 중심으로 테스트하기 쉽고 확장가능한 아키텍쳐를 제공한다. 우리는 실제 동작을 정의하는 부분을 작성하기만하면, NestJS가 정의한 데코레이터를 붙여서 서버에서 필요한 역할을 수행하는 각각의 요소들을 손쉽게 구축할 수 있다.

데코레이터는 트랜스파일러 환경에서 자바스크립트 클래스를 확장하기 위해 제안된 아직 실험적인 기능으로 ECMAscript의 stage-4는 Finished(종료), stage-3는 Candidate(후보), stage-2는 Draft(초안), stage-1은 Proposal(제안), stage-0은 Strawman(허수아비) 4단계 중 stage-2에 제안되어 있는 기능이다. 먼저 간단하게 소개하면 데코레이터라는 개념은 전달받은 것의 동작을 수정하여 새 것을 반환하는 함수라고 할 수 있으며, 데코레이터는 @을 붙여서 사용한다. 표현식이 여러번 선언될 경우 위에서 아래로 평가되고 결과는 아래에서 위로 호출된다.

공식 문서를 읽어보면 데코레이터는 다음과 같은 3가지 기능이 있다.

**1. 동일한 의미를 가진 일치하는 값으로 데코레이팅되는 값을 바꿀 수 있다.**  
**2. 메타데이터를 데코레이팅되는 값과 연결할 수 있다.**  
**3. 메타데이터를 통해 데코레이팅되는 값에 대한 엑세스를 제공할 수 있다.**

(메타데이터(metadata)는 데이터(data)에 대한 데이터이다. 이렇게 흔히들 간단히 정의하지만 엄격하게는, Karen Coyle에 의하면 "어떤 목적을 가지고 만들어진 데이터 (Constructed data with a purpose)"라고도 정의한다.) 기본적으로 데코레이터는 외부동작을 근본적으로 변경하지 않고 값을 메타프로그래밍하고 기능을 추가하는데 사용할 수 있다.

데코레이터는 다음 유형의 값에 적용될 수 있다.

**1. 클래스**  
**2. 클래스 필드(공개, 비공개 및 정적)**  
**3. 클래스 메서드(공개, 비공개 및 정적)**  
**4. 클래스 접근자(공개, 비공개 및 정적)**

데코레이터가 호출되면 두개의 매개변수를 받는다.

**1. 장식되는 값, 또는 undefined특별한 경우인 클래스 필드의 경우**  
**2. 데코레이트되는 값에 대한 메타데이터를 포함하는 컨텍스트 개체**

컨텍스트 개체는 데코레이트되는 값에 따라 달라진다.

`kind`: 장식된 값의 종류. 이것은 데코레이터가 올바르게 사용되었는지 확인하거나 다른 유형의 값에 대해 다른 동작을 갖도록 하는 데 사용할 수 있다. 다음 값 중 하나를 갖는다.  
`"class" | "method" | "getter" | "setter" | "field" | "auto-accessor"`  
`name`: 값의 이름, 또는 private 요소의 경우 값에 대한 설명 (예: 읽을 수 있는 이름).  
`access`: 값에 액세스하는 방법을 포함하는 개체, public 클래스 요소는 요소의 이름을 알면 외부에서 액세스할 수 있기 때문에 private 클래스 요소 에만 사용할 수 있다. 이 메서드는 또한 데코레이터에 전달된 현재 값이 아니라 인스턴스에 있는 private 요소 의 최종 값을 가져온다.  
`isStatic`: 값이 static클래스 요소 인지 여부 . 클래스 요소에만 적용.  
`isPrivate`: 값이 private class 요소인지 여부. 클래스 요소에만 적용.  
`addInitializer`: 데코레이터를 데코레이터로 호출한 경우 에 사용할 수 있다. @init:,  
`setMetadata`: 사용자가 정의 가능하며, 이 메타데이터를 통해 클래스에 엑세스 할 수 있다.

**클래스**

```javascript
type  ClassDecorator  =  ( value : Function ,  context : {
  종류 : "class" ;
  name : string  |  undefined ;
  addInitializer ? ( initializer : ( )  =>  void ) : void ;
  getMetadata ( key : symbol ) ;
  setMetadata ( key : symbol ,  값 : 알 수 없음 );
} )  =>  기능  |  무효 ;
```

**클래스 필드**

```javascript
type  ClassFieldDecorator  =  ( 값 : 정의되지 않음 ,  컨텍스트 : {
  종류 : "필드" ;
  이름 : 문자열  |  기호 ;
  액세스 ?: {  get ( ) : unknown ,  set ( 값 : unknown ) : void  } ;
  isStatic : boolean ;
  isPrivate : boolean ;
  addInitializer ? (이니셜라이저 : ( )  =>  무효 ) : 무효 ;
  getMetadata ( 키 : 기호 ) ;
  setMetadata ( 키 : 기호 ,  값 : 알 수 없음 ) ;
} )  =>  ( initialValue : 알 수 없음 )  =>  알 수 없음  |  무효 ;
```

**클래스 메서드**

```javascript
type  ClassMethodDecorator  =  ( 값 : 함수 ,  컨텍스트 : {
  종류 : "메서드" ;
  이름 : 문자열  |  기호 ;
  액세스 ?: {  get ( ) : 알 수 없음  } ;
  isStatic : boolean ;
  isPrivate : boolean ;
  addInitializer ? ( 이니셜라이저 : ( )  = >  무효 ) : 무효;
  getMetadata ( 키 : 기호 ) ;
  setMetadata ( 키 : 기호 ,  값 : 알 수 없음 ) ;
} )  =>  기능  |  무효 ;
```

**클래스 접근자**

```javascript
type ClassGetterDecorator = ( 값 : 함수 , 컨텍스트 : {
종류 : "getter" ;
이름 : 문자열 | 기호 ;
액세스 ?: { get ( ) : unknown } ;
isStatic : boolean ;
isPrivate : boolean ;
addInitializer ? ( 이니셜라이저 : ( ) = > 무효 ) : 무효;
setMetadata ( 키 : 기호 , 값 : 알 수 없음 ) ;
} ) => 기능 | 무효 ;

type ClassSetterDecorator = ( 값 : 함수 , 컨텍스트 : {
종류 : "setter" ;
이름 : 문자열 | 기호 ;
접근 ?: { set ( 값 : unknown ) : void } ;
isStatic : boolean ;
isPrivate : boolean ;
addInitializer ? ( 이니셜라이저 : ( ) => 무효 ) : 무효 ;
getMetadata ( 키 : 기호 ) ;
setMetadata ( 키 : 기호 , 값 : 알 수 없음 ) ;
} ) => 기능 | 무효 ;
```

<!-- ### 실제 사용

데코레이터를 타입스크립트와 함께 사용하기 위해서는 tsconfig.json에 experimentDecorators 컴파일러 옵션을 활성화 해야한다.
nest cli로 생성한 프로젝트의 tsconfig.json에서도 확인해볼 수 있다.

**tsconfig.json:**

```javascript
{
"compilerOptions": {
"target": "ES5",
"experimentalDecorators": true
}
}
```

 데코레이터 팩토리 함수는 데코레이터 함수를 감싸는 래퍼 함수입니다. 팩토리는 사용자로부터 전달 받은 인자를, 내부에서 반환되는 데코레이터 함수는 데코레이터로 사용됩니다.

```javascript
// 데코레이터 팩토리
function Component(value: string) {
  console.log(value)

  // 데코레이터 함수
  return function (target: Function) {
    console.log(target)
    console.log(target.prototype)
  }
}

// 데코레이터 팩토리를 사용하면 값을 전달할 수 있습니다.
@Component("tabs")
class TabsComponent {}

// TabsComponent 객체 생성
const tabs = new TabsComponent()
``` -->
<br>
<br>
<br>

## 의존성주입

<br>
<br>
<br>

참고  
[타입스크립트 핸드북](https://typescript-kr.github.io/pages/decorators.html)  
[제로초블로그](https://www.zerocho.com/category/ECMAScript/post/58ef998e177375001892f897)  
[에크마스크립트 공식 깃허브](https://github.com/tc39/proposal-decorators)
