---
title: "[번역과 공부 그 사이 어딘가]Solidity-Layout of a Solidity Source File"
date: "2022-01-26"
draft: false
path: "/blog/18"
---

처음 공부하면서 정리한 내용으로 틀린부분이 있을 수 있습니다. 주의

#### Storage Example
```
//SPDX-FileCopyrightText: © 2022 choichoikule email@gmail.com
//SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```

첫번째 라인은 소스코드가 General Publick License version 3.0 을 따른다는것을 알려주는 주석이다.
SPDX의 ID를 사용하면 코드의 라이선스를 기계가 이해할수 있는 방식으로 명확하게 알릴 수 있다.
기계가 이해할수 있다는 뜻은 컴파일러는 작성자가 적은 라이센스가 SPDX가 제공하는 유효한 라이센스 리스트에 포함되는지 판단한다는 뜻은 아니며, 다만 컴파일러가 컨트랙트 메타데이터를 담은 JSON파일을 생성할 때 해당 내용을 포함시킨다는 것을 의미한다. 아무런 라이센스도 명시하고 싶지않을경우 값은 UNLICENSED라고 적어주면 된다.
컴파일러는 언제 어디서든 해당 주석을 인식할수 있지만 파일의 첫번째 줄에 적는 것이 권장된다.  
 
두번째는 라인의 pragma라는 지시어는 어떤 버전의 컴파일러를 사용해야하는지 알려주는 역할을 한다. 프라그마 지시어는 항상 파일수준으로 작동하므로 모든 소스 파일에 명시해주어야한다. 버젼은 0.0.0 세자리로 표기하고 위의 코드 예시에서 처럼 부등호로 표현할 수도 있지만 ^표시로 특정 버전을 명시할 수 있기도 하다.

pragma 지시어는 컴파일러 버전을 명시하는 것 이외에도 Application Binary Interface 인코더/디코더를 무엇으로 구현할 것인지 선택할 수 있게한다. ```pragma abicoder v1``` 혹은 ```pragma abicoder v2``` 라고 작성하면 되는데 0.8.0 솔리디티 버전부터는 v2가 디폴트 값이므로 올드버젼을 사용하고 싶으면 명시적으로 작성해야한다. ABI는 데이터 구조와 함수가 어떻게 기계코드에서 사용되는지 그 방법을 정의하는 것으로 이더리움 생태계에서는 컨트랙트들과 상호작용하는 표준방법을 의미하는 것으로 생각 하면된다. 타입에 따라 데이터는 인코딩되고 스키마에따라 디코딩된다. 앞서 컴파일러가 컨트랙트 메타데이터 JSON파일을 생성한다고 했는데 이 JSON파일이 ABI로 사용되며 이에 따라 컨트랙트의 객체를 만들수 있고 함수를 호출할수 있게 된다. 좀더 상세한 설명은 그대로 인용한다. 
> ABI는 컨트랙트 내의 함수를 호출하거나 컨트랙트로부터 데이터를 얻는 방법이다. 이더리움 스마트 컨트랙트는 이더리움 블록체인에 배포된 바이트코드다. 컨트랙트 내에 여러 개의 함수가 있을 수 있을 것이다. ABI는 컨트랙트 내의 어떤 함수를 호출할지를 지정하는데 필요하며, 우리가 생각했던 대로 함수가 데이터를 리턴한다는 것을 보장하기 위해 반드시 필요하다.
abicoder v1과 v2의 차이점은 v2에서는 중첩된 배열과 구조체를 허용한다는 것이 다르다. 

솔리디티는 모듈화를 지원하기 위해 import 구문을 사용하는데 default export는 지원하지 않는다. ``` ipmort "filename"; ```와 같이 임포트해올경우 현재의 글로벌스코프에 포함되게 된다. 이경우 네임스페이스를 오염시킬 위험이 있으므로 특정심볼을 명시적으로 사용해서 다음과 같이 임포트해오는 것이 권장된다. ```import * as symbolName from "filename";```, ```import "filename" as symbolName;```, ```import {symbol1 as alias, symbol2} from "filename";```
솔리디티에서의 임포트 경로는 호스트의 파일시스템의 파일을 직접적으로 의미하지않는다. 솔리디티는 유니크한 소스 유닛 이름을 할당하여 virtual filesystem인 내부의 데이터베이스에 저장하고 import구문의 콜백으로 실제 파일을 찾아오는 식으로 작동한다. ```import "./util.sol"```와 같이 상대경로로 임포트해오는 방식을 사용할 수 있기도하다. ```./``` or ```../```로 시작해야한다.

 참고:  
 [소스 코드 내 저작권 표시를 해야 하는 이유와 올바른 방법](https://devocean.sk.com/opensource/techBoardDetail.do?ID=159339)  
 [스마트 컨트랙트와 솔리디티](https://imeom.tistory.com/172?category=468763)
 [솔리디티 공식문서](https://docs.soliditylang.org/)  
 [ABI와 관련된 Q&A정리](https://medium.com/pocs/ethereum-abi%EC%99%80-%EA%B4%80%EB%A0%A8%EB%90%9C-q-a-%EC%A0%95%EB%A6%AC-40e639ee1a03)  
 [VFS](https://richong.tistory.com/197)