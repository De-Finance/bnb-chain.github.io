---
sidebar_label: Overview
sidebar_position: 2
hide_table_of_contents: false
---
# Overview

BNB 스마트 체인은 비컨 체인(Beacon Chain)에 프로그램성과 상호운용성을 도입하기 위한 혁신적인 솔루션입니다. BNB 스마트 체인은 21명의 검증인과 [Proof of Staked Authority (PoSA) consensus](https://github.com/bnb-chain/whitepaper/blob/master/WHITEPAPER.md#consensus-and-validator-quorum)로 구성된 시스템을 기반으로 하고 있어 짧은 블록타임과 낮은 수수료를 구현할 수 있습니다. 가장 많이 스테이킹한 검증인 후보들이 검증인이 되어 블록을 생성합니다. 이중서명 식별과 슬래싱 로직으로 보안, 안전성, 체인 완결성이 보장됩니다.

BSC는 21명의 검증인을 더 늘려갈 것입니다. 예컨대 "후보"라고 칭해지는 20명의 비활성 검증인을 예비 인력에 포함시킬 것입니다.

후보들은 BSC 메인넷에서 블록을 생성하고 가스비를 받지만, 이는 21명의 선출된 공식 검증인 보다는 낮은 확률로 이루어집니다. 부재중인 후보들은 적은 수이긴 하지만 슬래싱됩니다. 후보 검증인들이 BSC의 품질과 안정성을 유지할 수 있기 위해서는 어느 정도의 의지가 필요합니다.

극단적인 경우 21명의 활동 검증인의 과반수가 공격을 받아 접속이 끊길 경우 후보 검증인들이 비컨 체인에 이 스테일 블록킹에 대해 보고하고, 재개한 뒤 활동 검증인 집단을 재선출하자는 제안을 올릴 수 있습니다.

## 검증인이 무엇인가요?

BNB 스마트 체인은 블록체인에 블록을 확정시키는 검증인 집단에 의존하고 있습니다. 이 검증인들은 블록에 개인키로 암호화된 서명을 하는 방식으로 합의 프로토콜에 참여합니다. 검증인 집단은 BNB 스마트 체인을 위해 비컨 체인에 만들어진 스테이킹 모듈을 통해 결정되며, 매일 UTC 00:00 BC에서 BSC로 크로스 체인 커뮤니케이션을 통해 전파됩니다.


## 에코노믹스

검증인의 보상은 트랜잭션 수수료와 위임자들의 커미션에서 비롯됩니다.

Let us also assume that the reward for a block is 100 BNB and that a certain validator has 20% of self-bonded BNB and sets its commission rate to 20%. These tokens do not go directly to the proposer. Instead, they are shared among validators and delegators.  These 100 BNB will be distributed according to each participant's stake:

```
Commission: 80*20%= 16 BNB
Validator gets: 100\*20% + Commission = 36 BNB
All delegators get: 100\*80% - Commission = 64 BNB
```

If validators double sign, are frequently offline, their staked BNB ( not including BNB of users that delegated to them) can be slashed. The penalty depends on the severity of the violation.

You can learn to see the revenue history from BitQuery's [chart](https://explorer.bitquery.io/bsc/miners) or a table of [BscScan](https://bscscan.com/validatorset)

## Risks for Validators

If you try to cheat the system, or act contrary to the specification, you will be liable to incur a penalty, known as **[slashing](bc-slashing.md)**.


### Potential Loss


#### Loss for Double-Sign Slash

Running your validator keys simultaneously on two or more machines will result in Double-Sign slashing.

Penalty for Double-Sign Slash:

1. 10000 staked BNB will be slashed for the validator.
2. The Double-Sign Jail time is 2^63-1 seconds, which means the candidate cannot become a validator anymore.

> Note: Rewards for submitting double-sign evidence: 1000BNB Anyone can submit a slashing request on BC with the evidence of Double Sign of BSC, which should contain the 2 block headers with the same height and parent block, sealed by the offending validator.


#### Loss for Offline Slash:


If a validator missed more than 50 blocks every 24h, the blocking reward for validator will not be relayed to BC for distribution but shared with other better validators. If it missed more than 150 blocks every 24h, then this will be propagated back to BC where another Slashing will happen

Penalty for Offline Slash:

1. 50 staked BNB will be slashed for the validator.
2. The Downtime Jail time is 2 days, which means the candidate can send a `unjail` transaction to become a candidate again.



#### Loss for Too Low self-delegation

Validator candidates must stake 10000BNB as self-delegation. If the self-delegation amount is lower, the Jail time is 1 day.
