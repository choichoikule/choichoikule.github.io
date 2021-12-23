---
title: "NFT만들기"
date: "2021-09-23"
draft: true
path: "/blog/post-6"
---

이더리움 재단에서 작성한 [NFT 만들기 포스팅](https://ethereum.org/en/developers/tutorials/how-to-write-and-deploy-an-nft/)를 읽으면서 작성한 글이다.
작성중

MyNFT.sol

```javascript
//Contract based on [https://docs.openzeppelin.com/contracts/3.x/erc721](https://docs.openzeppelin.com/contracts/3.x/erc721)
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
// 우리의 nft 스마트 컨트랙트가가 상속받을 ERC721 표준 이 표준을 따라야 valid 하기때문에
import "@openzeppelin/contracts/utils/Counters.sol";
// 우리가 제작한 nft의 토탈갯수를 세고 번호를 매김
import "@openzeppelin/contracts/access/Ownable.sol";
// 스마트 컨트랙트를 소유한 나만이 nft를 주조할 수 있다.
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";

contract MyNFT is ERC721URIStorage, Ownable {
    using Counters for Counters.Counter;
    Counters.Counter private _tokenIds;

    constructor() ERC721("CryptoGradient", "gradient") {}

    function mintNFT(address recipient, string memory tokenURI)
        public onlyOwner
        returns (uint256)
    {
        _tokenIds.increment();

        uint256 newItemId = _tokenIds.current();
        _mint(recipient, newItemId);
        _setTokenURI(newItemId, tokenURI);

        return newItemId;
    }
}

//recipient 는 새롭게 만든 nft를 받을 주소
//tokenURI는 nft의 메타데이러를 묘사하는 제이슨 문서로 변환가능한 스트링

```

alchemy는 블록체인 개발자 플랫폼이자 API이다. 우리의 노드가 이더리움 체인과 소통할 수 있게 해준다. 무료계정을 만든다.
롭슨 테스트넷에서 앱을 만든다.
그리고 메타마스크로 지갑을 생성한다.
faucet으로 부터 테스트용 이더를 발급받는다.
alchemy에서 일어난 결과를 확인할 수 있다.

프로젝트를 초기화한다.
hardhat을 설치한다.
hardhat은 이더리움 소프트웨어를 컴파일하고 디플로이하고 테스트 해볼 수 있는 개발 환경이다.
하드햇은 로컬 이더리움 네트워크인 하드햇네트워크에 기반한다. 하듷햇은 플러그인들로 가능을 사용하며 개발자가 원하는대로 선택하면 된다.

폴더를 두개 만든다. 컨트랙트 폴더는 nft를 저장하는 컨트랙트 코드가 있는곳이고
스크립트 폴더는 컨ㅌㅡ랙트와 상호작용하는 스크립트를 저장하는 곳이다.

토큰 표준은 ERC(Ethereum Request for Comment)에서 정의됩니다.
ERC721
ERC721은 NFT(대체 불가능 토큰)의 최상위 솔루션입니다. 다른 모든 토큰과 마찬가지로 NFT는 가상 및 물리적 자산의 소유권을 둘 다 나타냅니다. 관련 자산에는 다음이 포함될 수 있습니다.

수집 가능한 항목(예: 골동품, 카드 또는 미술품)
주택 또는 자동차와 같은 물리적 자산
대출과 같은 소극적 가치 자산
각 토큰은 고유하며 추적해야 하는 소유권 및 상태를 포함합니다.

마찬가지로 ERC721 토큰과 ERC20 토큰의 복잡성은 다릅니다. ERC721 토큰이 더 복잡합니다. 또한 ERC721 토큰을 사용하는 경우 각 함수에는 스마트 계약에서 사용되는 토큰을 고유하게 식별하는 토큰 ID를 지정하는 인수가 있습니다.

OpenZeppelin은 분산형 애플리케이션을 작성, 배포 및 관리하는 데 사용할 수 있는 도구를 포함하는 플랫폼입니다. OpenZeppelin은 플랫폼에서 제공하는 제품에 안정성과 보안을 제공하는 오픈 소스 도구입니다.

OpenZeppelin은 계약 라이브러리와 SDK라는 두 가지 제품을 제공합니다.

계약
OpenZeppelin 계약 라이브러리는 Ethereum 네트워크의 강력한 모듈식 및 재사용 가능한 스마트 계약 세트에 대한 액세스를 제공합니다. 스마트 계약은 Solidity로 작성됩니다. OpenZeppelin 계약 사용의 주요 이점은 철저하게 테스트 및 감사되고 커뮤니티에서 검토되었다는 것입니다.

OpenZeppelin은 스마트 계약 업계에서 가장 인기 있는 라이브러리 원본이며 오픈 소스입니다. OpenZeppelin 계약을 사용하는 경우 스마트 계약을 개발하기 위한 모범 사례를 알아봅니다. 다음을 비롯한 다양한 계약 형식을 사용할 수 있습니다.

액세스 제어: 작업을 수행할 수 있는 사용자를 결정할 때 사용합니다.
토큰: 거래 가능 자산을 만드는 데 사용합니다.
가스 스테이션 네트워크: 사용자가 가스 이용료(요금)를 납부하지 않고 계약을 사용할 수 있게 하려는 경우 사용합니다.
유틸리티: 일반적이고 유용한 도구가 필요한 경우 사용합니다.
이 모듈에서는 토큰 계약만 사용하지만 사용 가능한 다른 계약 리소스를 알아보는 것이 좋습니다.

SDK)
사용할 수 있는 다른 OpenZeppelin 제품은 OpenZeppelin SDK입니다. SDK는 CLI(명령줄 인터페이스)를 제공하므로 스마트 계약 개발을 더 쉽게 관리할 수 있습니다. CLI를 사용하여 스마트 계약을 컴파일, 업그레이드 및 배포하면 개발 시간을 단축할 수 있습니다. CLI는 Ethereum 및 기타 Ethereum 가상 머신 기반 블록체인에 대한 지원을 제공합니다. 명령은 개발 프로세스를 안내하는 데 도움이 되도록 직관적이고 대화형입니다.

이 모듈에서는 SDK를 사용하지 않지만, SDK는 직접 살펴보는 것이 좋고 향후 블록체인 개발에 사용할 수 있는 도구입니다.
<img src="https://steemitimages.com/DQmVTKkYB6JL15YmAmYDEbohFFAyoBgyXTd8HH8tka7EST5/image.png">

evm이란 이더리움 버추얼 머신을 가리킴

1. 일반 사용자
   위 그림 왼편을 보면 일반적인 사용자는 이더리움 블락체인 데이터를 가지고 있지 않습니다. 이더리움 블락체인 데이터는 수백 GB데이터 이며, 블락체인의 노드로 동작하기 위해서는 계속 블락데이터를 다운로드 받아 싱크를 맞춰야 합니다. 그래서 일반 사용자는 블락체인 데이터를 직접 저장하지 않고, 일종의 서버와 같은 역할을 하는 노드에 연결합니다. 이 때 웹 브라우저나 ssh와 같은 서비스를 이용하게 되는 것이죠.
2. 이더리움 클라이언트
   그림 중앙에 나타난 이더리움 클라이언트는 이더리움 블락체인에 참여하는 노드입니다. 즉 블락이 생성되면 그 정보를 전파 받는 역할을 하는 것이죠. 따라서, 싱크된 블락체인 데이터를 가지고 있어야 합니다. 이더리움 클라이언트는 블락체인 네트워크의 노드이면서, 일반 사용자의 접속을 허용하고 블락체인과 연결시켜주는 역할도 담당합니다. 그래서 일반 사용자는 이더리움 클라이언트, 즉 노드에 접속하여 geth와 같은 명령어로 블락체인 정보를 얻거나, 스마트 컨트랙트를 사용할 수 있습니다.

이더리움 클라이언트는 블락체인 데이터를 모두 가지고 있습니다. 한 사용자가 생성한 스마트 컨트랙트는 이더리움 클라이언트를 통해서 블락에 포함되고, 결국 블락체인에 연결됩니다.

스마트 컨트랙트가 블락체인에 포함된다는 얘기는 블락체인 네트워크 상의 모든 노드들이 동일한 스마트 컨트랙트(엄밀히는 스마트 컨트랙트 코드)를 가지고 있는 것입니다!
위 내용이 위 그림의 오른편에 나타나 있습니다. 한 사용자가 생성한 스마트 컨트랙트 코드가 블락에 기록되어 다른 노드에서도 코드가 복사된 것이죠. 그래서 각 노드의 EVM에서 코드를 실행시킬 수가 있습니다!

위 내용을 실제 예를 들어서 좀 더 구체적으로 알아보겠습니다. 자 따라오시면, 이더리움 스마트 컨트랙트 개념을 종결시킬 수 있습니다!

스마트 컨트랙트를 만드는 순서는 다음과 같습니다.

스마트 컨트랙트 코딩
: 구현하고자 하는 내용을 솔리디티나 다른 언어로 코딩합니다.
구현한 소스 코드를 컴파일
: 컴파일 결과 EVM 바이트 코드가 생성됩니다.
스마트 컨트랙트 배포
: 스마트 컨트랙트를 배포한다는 것은 컴파일된 EVM 코드를 하나의 트랜잭션 처럼 블락에 추가시켜 블락체인에 등록시키는 작업입니다.
: 소스 컴파일 -> EVM 바이트 코드 -> 구체적인 작업은 ABI 취득 -> ABI로부터 컨트랙트 객체 생성 -> 트랜잭션 생성하여 블락에 추가
: 마이너가 해당 블락을 채굴하게 되면 블락체인에 포함됨

솔리디티로 짠 코드는 솔리디티 컴파일러에 의해 기계어가 만들어집니다. 이 경우는 Ethereum Bytecode라고 불립니다. 이 바이트 코드가 바로 EVM에서 실행되는 것입니다. EVM은 하나의 기계, Machine입니다. 그런데 가상이죠. 가상머신이란 말은 많이 들어보셨죠? 윈도우즈 운영체제에서 리눅스 운영체제를 쓰고 싶을 때, VMWare같은 프로그램을 써서 리눅스 가상 머신을 만들어서 사용하듯이, 보통 하드웨어나 소프트웨어 구조가 다른 것을 가상으로 만들어서 사용하는 것이 가상 머신입니다.
그럼 EVM은 뭘까요?
EVM은 Ethereum 블락체인 네트워크의 노드들이 공유하는 하나의 가상 머신입니다. 매우 거대한 하나의 분산 컴퓨터라고 보시면 됩니다. 블락체인 네크워크상의 노드들은 이 거대한 컴퓨터에 접근할 수가 있습니다. 컴퓨터가 저장하고 있는 상태도 즉시 바꿀 수도 있고요. 그런데 말입니다. 전세계 많은 사람들이 동시에 EVM의 동일한 상태를 변경하면 어떻게 될까요? 분명 데이터의 충돌이 일어날 것입니다. 우리가 다른 사람들과 공유 문서를 사용할 때도 동일한 부분을 서로 다르게 고치면 충돌이 일어나듯이 EVM 데이터도 충돌이 일어날 수 밖에 없습니다. 이것을 중재하는 것이 EVM의 중요 역할이 됩니다. 이 역할이 마이닝과 합의 알고리듬으로 해결되는 것이고요.누구나 사용할 수 있는 EVM은 공짜가 아닙니다. 공짜라면 무수히 많은 사적인? 쓸모없는? 스팸성의 코드들이 넘쳐날 것입니다. EVM을 사용하려면 비용을 지불해야 합니다. 그 비용은 단순히 트랜잭션의 크기에 비례하는 것이 아니라, 실제로 트랜잭션을 실행하는데 필요한 비용입니다. 이 때 사용되는 개념이 까스 까스 까스 Gas입니다. 한가지 분명히 할 것은 Gas는 화폐가 아닙니다. 즉 트랜잭션 처리하기 위한 Gas가 100이라면 이것을 바로 화폐가치를 가지는 Ether로 환산할 수 없습니다. 왜냐하면 Gas는 일의 양을 나타내는 단위 같은 것입니다. 값어치를 갖지 않는 것이지요. 아주 단순히 예를 들면 트랜잭션을 수행하기 위해 Loop를 100번 돌려야한 한다라는 것을 의미한다고 할 수 있겠습니다. 그럼 EVM이 트랜잭션을 실행하기 위해 필요한 비용은 이렇게 계산됩니다.

현재 블록체인에서 사용되고 있는 스마트 컨트랙트는, 서면으로 이루어지던 계약을 코드로 구현하고 특정 조건이 충족되었을 때 해당 계약이 이행되도록 하는 script에 가깝습니다.

참고:
[해시넷](http://wiki.hash.kr/index.php/%EC%9D%B4%EB%8D%94%EB%A6%AC%EC%9B%80)
[스마트컨트랙트의 개념](https://steemit.com/coinkorea/@etainclub/smart-contract-6-dapp)
[EVM](https://opentutorials.org/course/2869/18360)