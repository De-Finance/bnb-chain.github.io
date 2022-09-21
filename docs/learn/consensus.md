---
sidebar_label: Consensus Engine
sidebar_position: 2
hide_table_of_contents: false
---

# BNB 스마트체인의 합의 엔진

## 요약
BSC(BNB 스마트 체인)의 합의 엔진은 다음의 목표를 위해 구상되었습니다:

1. 몇 개의 블록이 확정될 때까지 대기(이더리움 1.0 보다 적어야 함), 대부분의 경우 포크가 없는 편이 나음.
2. 이더리움 1.0 대비 블로킹 시간이 짧아야 함(5초 이하).
3. 인플레이션 없음. 트랜잭션 가스비가 블록 보상.
4. 이더리움 정도의 호환성.
5. 코스모스만큼 강력한 스테이킹과 거버넌스.


[Geth](https://github.com/ethereum/go-ethereum/wiki/geth)는 두 종류의 합의 엔진을 구현하고 있습니다: ethash(PoW 기반) 그리고 [clique](https://ethereum-magicians.org/t/eip-225-clique-proof-of-authority-consensus-protocol/1853)(PoA 기반)입니다. BSC는 PoW를 포기했기 때문에 Ethash는 적절한 선택지가 아닙니다. Clique는 블로킹 타임이 더 짧고, 51% 어택에 취약하지 않은 동시에 기존 이더리움 클라이언트 호환성을 유지하기 위해 코어 데이터 구조에 최소한의 변화만을 주었습니다. PoA의 단점은 중앙화, 유의미한 스테이킹의 부재 그리고 온체인 거버넌스 능력의 부족입니다. 반면에 비컨 체인은 스테이킹 및 거버넌스 메커니즘을 보유하고 있는 코스모스 위에 만들어졌습니다. 따라서 다음과 같은 합의 엔진을 제안하려 합니다:

* 비컨 체인이 BSC 대신 스테이킹과 거버넌스를 담당합니다.
* 검증인 집단은 변화하며, BSC의 이중 서명 슬래싱은 체인 간 커뮤니케이션을 통해 업데이트 됩니다.
* BSC의 합의 엔진은 clique 만큼 단순합니다.

대표적인 PoA 합의 몇 가지를 조사해본 결과 [Bor](https://blog.polygon.technology/heimdall-and-bor/)가 위와 흡사하게 설계되었음을 발견했습니다. Bor로부터 몇 부분을 차용하여 위의 모든 목표를 달성할 수 있는 새로운 합의 엔진을 제안할 것입니다.

## 인프라 구성 요소

1. **비컨 체인(Beacon Chain)**. BSC 검증인을 독립된 투표를 통해 스테이킹 기능을 실현합니다. 투표 워크 플로우는 스테이킹 절차에 의해 실행됩니다.
2. **BSC 검증인**. 검증인들은 트랜잭션을 검증하고 블록을 생성함으로써 네트워크의 안보와 원장의 일관성을 보장합니다. 그 대신 트랜잭션의 가스 소비로부터 보상을 받습니다.
3. **BSC(또는 시스템 컨트랙트)의 스테이킹 dApp**. BSC에서 스테이킹을 구현하기 위한 제네시스 컨트랙트들이 있습니다. 여섯 가지로 분류가 됩니다:
    - **Light client 컨트랙트**. 오직 비컨 체인의 합의 알고리즘을 검증하는 컨트랙트에 의해 구현된 분산 합의 프로세스의 watcher입니다.
    - **Cross Chain 컨트랙트**. 크로스체인 커뮤니케이션 레이어입니다. 크로스체인 패키지의 순서와 머클 증명(merkle proof)를 검증할 것입니다.
    - **BSCValidatorSet 컨트랙트**. 비컨 체인 상 BSC의 검증인 변경을 감시합니다. BSC의 검증인 집단 변경을 적용할 것입니다. 또한 검증인들을 위한 블록킹 가스비 보상을 저장하며, It will apply the validator set change for BSC. It also stores rewarded gas fee of blocking for validators, and distribute revenue to validators when receiving cross chain package of validatorSet change.
    - **System Reward 컨트랙트**. 릴레이어들이 시스템 컨트랙트들을 유지할 수 있도록 해주는 인센티브 메커니즘입니다. 시스템 보상 컨트랙트에서 보상을 받을 것입니다.
    - **Liveness Slash 컨트랙트**. BSC의 liveness는 검증인 세트에 의존하며, 자신의 차례에 적시에 블록을 생성할 수 있습니다. 검증인들은 어떤 이유에서든 자신의 차례를 놓칠 수 있습니다. 작업의 이러한 불안정성은 네트워크의 성능에 악영향을 미치며, 더 비결정론적인 성질을 시스템에 도입합니다. 이 컨트랙트는 각 검증인의 누락된 블록킹 메트릭을 기록하는 역할을 합니다. 메트릭들이 사전에 정의된 임계치 이상이라면, 검증인을 위한 블로킹 Once the metrics are above the predefined threshold, the blocking reward for validator will not be relayed to BC for distribution but shared with other better validators.
    - **기타 contracts**.  BSC may take advantage of powerful governance of Beacon Chain, for example, propose to change a parameter of system contracts.

비컨 체인의 스테이킹과 거버넌스는 합의 보다 상위에 있는 레이어입니다. 릴레이어의 경우 독립적인 프로세스이며, 구현 방법은 아직 미정입니다. 이에 대한 자세한 내용은 이 문서에서는 다루지 않겠습니다.

이 문서는 오직 합의 엔진과 더 긴밀하게 연관이 있는 **BSC 검증인**와 **스테이킹 dApps**에만 초점을 맞출 것입니다.

## 시스템 보상 분배
BSC의 시스템 보상 구조는 유연하게 변동할 수 있습니다. 거버넌스를 통해 파라미터를 조정할 수 있습니다.

보상들은 트랜잭션 수수료에서 비롯됩니다. 보상은 몇 개의 (조정 가능한) 규칙에 기반하여 분배됩니다:
1. 블록을 생성하는 검증인들은 가스비의 15/16를 받습니다.
2. 시스템 보상 컨트랙트는 가스비의 1/16를 받습니다.

시스템 보상 컨트랙트의 잔고가 100BNB 이상인 경우, BNB를 분배하지 않습니다.
아래에서는 이 컨트랙트들이 보상을 어떻게 분배하는지 설명하겠습니다.

## BSC의 스테이킹 dApps

### [BSCValidatorSet contract](https://bscscan.com/address/0x0000000000000000000000000000000000001000)
비컨 체인 상에서 BSC의 검증인 변경의 감시자입니다. 아래와 같은 인터페이스를 구현합니다:

- **handleSynPackage(uint8, bytes calldata msgBytes)**

**Conditions**:
        1. 메시지 Sender는 CrossChainContract를 사용합니다.

**Action**:
        1. msgBytes의 첫 번째 바이트가 0x00, do Actions validators update;
        2. if the first byte of msgBytes is 0x01, do Actions jail.

**Actions jail**:
        1. mark the validator as jailed.

**Actions validators update**:

        1. Do distribue the revenue of validators:
        if the revenue is large than 0.1 BNB, will do cross chain transfer to its account on BC, otherwise will transfer to its address on BSC.
        2. Update the latest validatorSet.
        3. Clean the metrics record on slash contract.

**CurrentValidator() returns ([]address)**

    returns the the consensus address of not jailed validators.

**deposit(address valAddr) external**

**Conditions**:

        1. The message sender must be the coinbase
        2. Can only call once in one block.

**Actions**:

        1. Increase the revenue of the validator.

### [System Reward contract](https://bscscan.com/address/0x0000000000000000000000000000000000001002)
For now, only **Cross Chain contract** is permitted to call system reward contract. It implement the following interfaces:

- **claimRewards(address payable to, uint256 amount) external**

    **Conditions**:

        1. The message sender must in permission list.
        2. The amount should be no more than 1 BNB

    **Actions**:

        1. Transfer amount of BNB to specified address

### [Liveness Slash contract](https://bscscan.com/address/0x0000000000000000000000000000000000001001)
If a validator failed to produce a block, will record it and finally slash it. It implement the following interfaces:

- **Slash(validator address) external**

    **Conditions**:

        1. The message sender must in coinbase.
        2. can only call once in one block.

    **Actions**:

        1. increase the missing blocks metrics of the validator by one.
        2. if the missing blocks metrics is times of 50, will call misdemeanor func of BSCValidatorSet contract to trigger a misdemeanor event and distribute the revenue of the validator to others.
        3. if the missing blocks metrics is times of 150, will call felony func of BSCValidatorSet contract to trigger a felony event, not only distribute the revenue of the validator to others, but also kick the validator out of validatorset.


## Consensus Protocol

The implement of the consensus engine is named as **Parlia** which is similar to [clique](https://ethereum-magicians.org/t/eip-225-clique-proof-of-authority-consensus-protocol/1853). This doc will focus more on the difference and ignore the common details.

Before introducing, we would like to clarify some terms:

1. Epoch block. Consensus engine will update validatorSet from BSCValidatorSet contract periodly.  For now the period is 200 blocks, a block is called epoch block if the height of it is times of 200.
2. Snapshot.  Snapshot is an assistant object that help to store the validators and recent signers of blocks.


### 핵심 기능

#### Light Client Security
Validators set changes take place at the (epoch+N/2) blocks. (N is the size of validatorset before epoch block). Considering the security of light client, we delay N/2 block to let validatorSet change take place.

Every epoch block, validator will query the validatorset from contract and fill it in the extra_data field of block header. Full node will verify it against the validatorset in contract. A light client will use it as the validatorSet for next epoch blocks, however, it can not verify it against contract, it have to believe the signer of the epoch block. If the signer of the epoch block write a wrong extra_data, the light client may just go to a wrong chain. If we delay N/2 block to let validatorSet change take place, the wrong
epoch block won’t get another N/2 subsequent blocks that signed by other validators, so that the light client are free of such attack.

#### System Transaction
The consensus engine may invoke system contracts, such transactions are called system transactions. System transactions is signed by the the validator who is producing the block. For the witness node, will generate the system transactions(without signature) according to its intrinsic logic and compare them with the system transactions in the block before applying them.

#### Enforce Backoff
In Clique consensus protocol, out-of-turn validators have to wait a randomized amount of time before sealing the block. It is implemented in the client-side node software and works with the assumption that validators would run the canonical version.
However, given that validators would be economically incentivized to seal blocks as soon as possible, it would be possible that the validators would run a modified version of the node software to ignore such a delay. To prevent validator rushing to seal a block, every out-turn validator will get a specified time slot to seal the block. Any block with a earlier blocking time produced by a out-turn validator will be discarded by other witness node.

### How to Produce a New Block

#### Step 1: Prepare
A validator node prepares the block header of next block.

* Load snapshot from cache or database,
* If (height % epoch)==0, should fetch ValidatorSet from `BSCValidatorSet` [contract](https://bscscan.com/address/0x0000000000000000000000000000000000001000).
*  Every epoch block, will store validators set message in `extraData` field of block header to facilitate the implement of light client.
* The coinbase is the address of the validator

#### Step2: Finalize And Assemble

* If the validator is not the in turn validator, will call liveness slash contract to slash the expected validator and generate a slashing transaction.
* If there is gas-fee in the block, will distribute **1/16** to system reward contract, the rest go to validator contract.

#### Step3: Seal
The final step before a validator broadcast the new block.

* Sign all things in block header and append the signature to extraData.
* If it is out of turn for validators to sign blocks, an honest validator it will wait for a random reasonable time.

### How to Validate/Replay a block

#### Step1: Verify Header
Verify the block header when receiving a new block.

* Verify the signature of the coinbase is in `extraData` of the `blockheader`
* Compare the block time of the `blockHeader` and the expected block time that the signer suppose to use, will deny a `blockerHeader` that is smaller than expected. It helps to prevent a selfish validator from rushing to seal a block.
* The `coinbase` should be the signer and the difficulty should be expected value.

#### Step2: Finalize

* If it is an epoch block, a validator node will fetch validatorSet from BSCValidatorSet and compare it with extra_data.
* If the block is not generate by inturn validatorvalidaror, will call slash contract.
if there is gas-fee in the block, will distribute 1/16 to system reward contract, the rest go to validator contract.
* The transaction generated by the consensus engine must be the same as the tx in block.

### Signature
The signature of the coinbase is in extraData of the blockheader, the structure of extraData is:
epoch block. 32 bytes of extraVanity + N*{20 bytes of validator address} + 65 bytes of signature.
none epoch block. 32 bytes of extraVanity + 65 bytes of signature.
The signed content is the `Keccak256` of RLP encoded of the block header.


### Security and Finality
Given there are more than 1/2\*N+1 validators are honest, PoA based networks usually work securely and properly. However, there are still cases where certain amount Byzantine validators may still manage to attack the network, e.g. through the “Clone Attack”. To secure as much as BC, BSC users are encouraged to wait until receiving blocks sealed by more than 2/3\*N+1 different validators. In that way, the BSC can be trusted at a similar security level to BC and can tolerate less than 1/3\*N Byzantine validators.

With 21 validators, if the block time is 5 seconds, the 2/3\*N+1 different validator seals will need a time period of (2/3\*21+1)\*5 = 75 seconds. Any critical applications for BSC may have to wait for 2/3\*N+1 to ensure a relatively secure finality. However, besides such an arrangement, BSC does introduce Slashing logic to penalize Byzantine validators for double signing or instability. This Slashing logic will expose the malicious validators in a very short time and make the [Clone Attack](https://arxiv.org/pdf/1902.10244.pdf) very hard or extremely non-economic to execute. With this enhancement, 1/2\*N+1 or even fewer blocks are enough as confirmation for most transactions.

### Potential Issue

#### Extending the ruling of the current validator set via temporary censorship
If the transaction that updates the validator is sent to the BSC right on the epoch period, then it is possible for the in-turn validator to censor the transaction and not change the set of validators for that epoch. While a transaction cannot be forever censored without the help of other n/2 validators, by this it can extend the time of the current validator set and gain some rewards. In general, the probability of this scheme can increase by colluding with other validators. It is relatively benign issue that a block may be approximately 5 secs and one epoch being 240 blocks, i.e. 20 mins so the validators could only be extended for another 20 mins.
