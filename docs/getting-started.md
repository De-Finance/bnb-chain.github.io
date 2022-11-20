---
sidebar_label: Getting Started
---

# 시작하기
본 튜토리얼의 목적은 BNB 체인에 대한 전반적인 개요를 제공하고 BNB 체인 생태계에 대한 신규 사용자의 출발점 역할을 하는 것입니다. 암호화폐에 대한 일반적인 지식, 특히 이더리움 생태계에 대한 익숙하다는 것을 가정합니다. 만약 여러분이 무언가를 바로 이해하지 못한다면 괜찮습니다. 온라인에서 검색하고 답변을 찾을 수 없으면 [Discord](http://discord.com/invite/bnbchain)에서 문의하십시오.


BNB 체인을 사용하기 전에 이 문서를 완전히 읽는 것이 좋습니다. 그러면 새로운 사용자가 직면하는 일반적인 함정과 문제를 피할 수 있습니다. BNB 체인에는 여러 구성 요소가 있으므로 혼란을 피하기 위해 시작하기에 앞서 모든 것을 파악하는 것이 가장 좋습니다. 

BNB 체인에 대한 전반적 개요를 [여기](bnbIntro)에서 확인할 수 있습니다. BNB 체인과 다른 플랫폼 간의 유사점과 차이점을 이해하는 데 유용할 것입니다.

## BNB 체인: 비콘 체인과 BNB 스마트 체인
BNB 체인은 비콘 체인(BC)과 BNB 스마트 체인(BSC)이라는 두 개의 블록 체인으로 구성되어 있습니다. [비콘 체인](learn/beaconIntro.md)는 BNB 체인의 거버넌스를 담당하고 BNB 체인에 대한 지분 및 투표를 관리하는 블록체인 컴포넌트입니다. 반면, [BNB 스마트 체인](learn/intro.md)은 EVM과 호환되며, 합의 계층이며 멀티체인 허브를 갖춘 블록체인 컴포넌트입니다.

### 비콘 체인(BC)으로 무엇을 할 수 있나요?
비콘 체인의 목적은 DEX를 따라 작동하는 효율적인 블록체인 생태계를 제공하여 탈중앙화된 방식으로 디지털 자산을 발행하고 교환할 수 있는 대체 시장을 제공하는 것입니다.

다음을 수행할 수 있습니다.

- [BNB 송수신](beaconchain/wallet/../transfer.md#web-wallet)
- [자산 디지털화를 위한 새로운 토큰 발행](https://community.binance.org/topic/2487), 및 비콘 체인을 자산의 기본 교환/전송 네트워크로 사용
- 토큰 송신, 수신, [소각](beaconchain/tokens.md#burn)/[발행](beaconchain/tokens.md#mint), [동결/동결 해제](beaconchain/tokens.md#freeze-unfreeze)
- [BNB 스마트체인을 위한 온체인 거버넌스 제안서 제출](learn/bsc-gov.md)


**트레이더*의 경우 다음을 수행할 수 있습니다.

- 두 개의 다른 토큰 [거래 페어 생성 제안](beaconchain/list_instruction.md)
- [SDK](beaconchain/exchange-integration.md#sdks)를 통해 체인에서 생성된 트레이딩 페어를 구매하거나 매매하기 위해 [오더 발송](beaconchain/learn/trading-interface.md)
- 특정 자산의 가격 및 시장 활동 확인을 우한 [DEX 시장 감시](beaconchain/develop/api-reference/dex-api/paths.md#apiv1markets)

**개발자*의 경우 다음을 수행할 수 있습니다.

- [BNB 체인 탐색기](https://explorer.bnbchain.org/), [API](beaconchain/develop/api-reference/dex-api/paths.md)와 [node RPC](beaconchain/develop/api-reference/node-rpc.md) 인터페이스를 통한 체인의 트랜잭션 기록 및 블록을 탐색
- [풀노드](beaconchain/fullnode.md)를 실행하여 트랜잭션, 블록 및 합의 활동에 대한 라이브 업데이트를 주시 및 전파
- 풀노드 또는 [API](beaconchain/develop/api-reference/dex-api/paths.md#apiv1markets)를 통한 바이낸스 체인의 다른 데이터를 추출
- 사용자가 바이낸스 체인 및 바이낸스 DEX를 사용할 수 있도록 도와주는 [도구](Integrate.md#sdks) 및 애플리케이션 개발

### BNB 스마트 체인(BSC)으로 무엇을 할 수 있습니까?

BNB 스마트 체인(BSC)은 비콘 체인과 병렬로 실행되는 블록 체인으로 가장 잘 설명할 수 있습니다. 비콘 체인과 달리 BSC는 스마트 컨트랙트 기능과 이더리움 가상 머신(EVM)과의 호환성을 자랑합니다. 설계 목표는 비콘체인의 높은 처리량을 그대로 유지하는 동시에 스마트 계약을 생태계에 도입하는 것이었습니다.

BSC는 EVM과 호환되므로 [Ethereum](https://academy.binance.com/en/articles/what-is-ethereum) 도구 및 DApp의 풍부한 환경을 지원하며 출시되었습니다. 이론적으로 이것은 개발자들이 이더리움에서 프로젝트를 쉽게 포팅할 수 있게 해줍니다. 즉, 사용자는 [메타마스크](wallet/metamask.md)와 같은 애플리케이션을 BSC와 함께 간단히 사용할 수 있습니다. 몇 가지 설정만 조정하면 됩니다. 시작하려면 [BNB 스마트 체인에서 메타마스크 사용하기](wallet/metamask.md)을 확인하십시오.

다음을 수행할 수 있습니다.

- 체인 간 [BNB](binance.md#transfer-testnet-bnb-from-bsc-to-bc) 및 기타 [BEP2 토큰](binance.md#swap-testnet-bep2-token-to-its-bep20-equivalent) 거래
- [bscscan](https://bscscan.com), API, 노드 RPC 인터페이스를 통한 거래 내역 및 블록을 탐색
- [BNB 스테이킹](wallet/staking.md)을 통한 블록 보상 획득

**개발자**는 또한 다음을 수행할 수 있습니다.

- 자산 디지털화를 위한 새로운 토큰 [발행] (issue-BEP20.md)
- 기존 DApps [이동](https://github.com/bnb-chain/bsc-develop-ecosystem)
- [풀노드](validator/fullnode.md) 실행하여 트랜잭션, 블록 및 합의 활동에 대한 실시간 업데이트 주시 및 전파
- [테스트넷](validator/guideline-testnet.md) 및 [메인넷](validator/guideline-mainnet.md)의 검증자 되기
- 사용자가 Dapp을 사용할 수 있도록 도와주는 [지갑](wallet/wallet_api.md) 및 도구 개발

## 지갑
BNB 비콘과 BNB 스마트체인을 사용하는 가장 쉬운 방법은 해당 체인에서 계정 및 전송을 지원하는 지갑을 사용하는 것입니다.

BNB 체인은 [https://www.bnbchain.org/en](https://www.bnbchain.org/en))에서 웹 지갑을 제공합니다. BNB 체인은 테스트넷용 웹 월렛도 [https://testnet.binance.org](https://testnet.binance.org)에서 제공합니다. 두 가지 모두 아래에 설명된 기능을 제공합니다.

- 지갑의 기본 역할을 하는 암호화 키 및 주소를 생성
- 주소의 자산 잔액을 표시
- 자산을 보내고 받기

Binance Extension Wallet also presents a trading UI, where you can examine market data and manage your orders to trade among the listed assets. BNB Smart Chain supports several popular wallets like [MetaMask](wallet/metamask.md) and [TrustWallet](wallet/trustwallet.md), to learn more about the supported wallets refer [here](Wallet.md). For a list of tutorials on how to use other supported wallets with BNB Smart Chain, refer [here](wallets/wallet-tutorial-overview.md).

## BNB Token and Fees
BNB is the cryptocurrency coin that powers the BNB Chain ecosystem. As one of the world's most popular utility tokens, not only can you trade BNB like any other cryptocurrency, you can also use BNB in a wide range of applications and use cases. 

Although initially based on the Ethereum network, the ERC-20 BNB tokens were later swapped with [BEP-2](https://academy.binance.com/en/glossary/bep-2) BNB at a 1:1 ratio. The BEP-2 BNB is the native coin of the Beacon Chain, and the mainnet.
In 2020, the BNB Smart Chain (BSC) was launched. BSC is a blockchain network that runs in parallel with the BNB Beacon Chain. This means that BNB can be found in three different forms:

- BNB BEP-2 on the BNB Beacon Chain.
- BNB BEP-20 on the BNB Smart Chain.
- BNB ERC-20 on the Ethereum network.

## How to Buy BNB Tokens
As all fees on BNB Chain are paid in BNB, therefore, in order to interact with the BNB Chain network you will need to have some BNB tokens. 
- BNB tokens can also be received for usage on testnet through the [testnet faucet](https://testnet.binance.org/faucet-smart).
- The BNB tokens for usage on mainnet are available for purchase on multiple leading exchanges and wallets as explained [here](#wallet). You can also refer [here](wallets/wallet-tutorial-overview) for tutorials on how to use different wallets for use with BNB Chain to send/receive/purchase BNB Tokens.

## Chain Explorer
Chain Explorer provides a portal to explore blocks and transaction details. On [BNB Chain Explorer](https://explorer.bnbchain.org/) and [BscScan](https://bscscan.com/), you can also check different asset types, the distribution of their ownerships, and owners' transactions.

## REST API
There are [Accelerated Nodes](beaconchain/develop/node/nodetypes.md) which provide advanced API services for the public. Here is a list of all the Rest API information Accelerated Node provides: [paths](beaconchain/develop/api-reference/dex-api/paths). For information regarding RPC Endpoints for BSC, refer [here](rpc.md).

### Node RPC
There are data seed nodes in the network which allow users to perform low-level operations like executing ABCI queries, viewing network/consensus state or broadcasting a transaction.
If you run a full node by yourself, you can also use those RPC functions. Here is a list of all the Node RPC services it provides: for Beacon Chain refer [here](beaconchain/develop/api-reference/node-rpc.md) and for BNB Smart Chain refer [here](rpc.md).

## Advanced Ways To Use BNB Chain
### Run your own full node

Please refer to this guide about how to run your own full node on [Beacon Chain](beaconchain/fullnode.md) and [BNB Smart Chain](validator/fullnode.md).

### Run your own Light Client

Please refer to this guide about [how to run your own light client on Beacon Chain](beaconchain/light-client.md).

### Access via Node Command Line Interface (CLI)

A Command Line Interface is available for Linux and Mac platforms. Please refer to the [CLI Reference](beaconchain/develop/api-reference/cli).

### Use SDKs

SDKs are also provided as a starting point for your apps.

There are two advanced SDK solutions for Beacon chain: [Java](<https://github.com/bnb-chain/java-sdk>) and [Golang](<https://github.com/bnb-chain/go-sdk>).

Both solutions provide functions for:

* Create wallets and manage keys
* Encode/sign transactions and submit to Binance Chain/DEX, including Transfer, New Order, Cancel Order, etc.
* Communicate with Binance Chain/DEX Node RPC calls through public node RPC services or your own private full nodes

Please refer to specific SDK documentation for more information:

- [Go](https://github.com/bnb-chain/go-sdk)([Documentation](https://github.com/bnb-chain/go-sdk/wiki))
- [Java](https://github.com/bnb-chain/java-sdk)([Documentation](https://github.com/bnb-chain/java-sdk/wiki))
- [Javascript](https://github.com/bnb-chain/javascript-sdk) ([Documentation](https://github.com/bnb-chain/javascript-sdk/wiki))
- [C++](https://github.com/bnb-chain/cplusplus-sdk)([Documentation](https://github.com/bnb-chain/cplusplus-sdk/wiki))
- [C#](https://github.com/bnb-chain/csharp-sdk)([Documentation](https://github.com/bnb-chain/csharp-sdk))
- [Python](https://github.com/bnb-chain/python-sdk)([Documentation](https://github.com/bnb-chain/python-sdk))
- [Swift](https://github.com/bnb-chain/swift-sdk)([Documentation](https://github.com/bnb-chain/swift-sdk/blob/master/README.md))


## Blockchain Details
Please check the [technical details](learn/beaconIntro.md#technology-details) for more technical information.
