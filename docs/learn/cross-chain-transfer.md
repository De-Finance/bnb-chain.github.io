# Cross-Chain Transfer Token Transfer

크로스체인 전송은 BC에서 bound BEP2나 BEP8 토큰 그리고 BSC에서 BEP20 토큰을 지원합니다.

## 토큰 정보 확인

첫째로, 이미 bound되어 있는 것을 확인해야 합니다. 예를 들어 **BNB**의 binding 정보를 확인할 수 있습니다:


```shell
## mainnet
bnbcli token info --symbol BNB --trust-node --node http://dataseed4.binance.org:80

## testnet
tbnbcli token info --symbol BNB --trust-node --node http://data-seed-pre-0-s3.binance.org:80 
```

```json
{
  "type": "bnbchain/Token",
  "value": {
    "name": "Binance Chain Native Token",
    "symbol": "BNB",
    "original_symbol": "BNB",
    "total_supply": "200000000.00000000",
    "owner": "tbnb1l9ffdr8e2pk7h4agvhwcslh2urwpuhqm2u82hy",
    "mintable": false,
    "contract_address": "0x0000000000000000000000000000000000000000",
    "contract_decimals": 18
  }
}
```

BNB가 양 체인의 네이티브 토큰인 만큼, `0x0000000000000000000000000000000000000000`를 상응하는 컨트랙트 주소로 사용합니다. 이에 더해 BSC에서는 네이티브 토큰의 decimals이 18인 반면, BC의 decimals은 8입니다. 따라서 1e8:BNB를 BSC로 전송할 경우, 수신자의 잔액은 1e18 만큼 증가합니다.


## BC에서 BSC로 BNB 전송하기

**Example:**

```shell
## mainnet
bnbcli bridge transfer-out --to 0xEe9546E92e6876EdF6a234eFFbD72d75360d91f0 --expire-time 1597543193 --chain-id Binance-Chain-Tigris --from owner --amount 100000000:BNB --node http://dataseed4.binance.org:80

## testnet
tbnbcli bridge transfer-out --to 0xEe9546E92e6876EdF6a234eFFbD72d75360d91f0 --expire-time 1597543193 --chain-id Binance-Chain-Ganges --from owner --amount 100000000:BNB --node http://data-seed-pre-0-s3.binance.org:80
```

Result:

```bash
Committed at block 465899 (tx hash: 68FFF82197E27A3EC14AFF8C99A035FA9CA7120312AA55E98D11DFC0F8D9F3B9, response: {Code:0 Data:[] Log:Msg 0:  Info: GasWanted:0 GasUsed:0 Events:[{Type: Attributes:[{Key:[84 114 97 110 115 102 101 114 79 117 116 83 101 113 117 101 110 99 101] Value:[49 49] XXX_NoUnkeyedLiteral:{} XXX_unrecognized:[] XXX_sizecache:0} {Key:[69 120 112 105 114 101 84 105 109 101] Value:[49 53 57 55 53 52 51 49 57 51] XXX_NoUnkeyedLiteral:{} XXX_unrecognized:[] XXX_sizecache:0} {Key:[97 99 116 105 111 110] Value:[99 114 111 115 115 84 114 97 110 115 102 101 114 79 117 116] XXX_NoUnkeyedLiteral:{} XXX_unrecognized:[] XXX_sizecache:0}] XXX_NoUnkeyedLiteral:{} XXX_unrecognized:[] XXX_sizecache:0}] Codespace: XXX_NoUnkeyedLiteral:{} XXX_unrecognized:[] XXX_sizecache:0})
```

## Transfer BNB from BSC to BC

### transferOut

Call **transferOut** of [TokenHub contract](https://raw.githubusercontent.com/bnb-chain/bsc-genesis-contract/master/abi/tokenhub.abi) in [MyEtherWallet](https://www.myetherwallet.com/):

<img src="https://lh3.googleusercontent.com/q8-nnt12h8gvYyMe6iwLalwzY-1jHfQ11BsSyIz3qkQPCjp_-D-dIzPxZ-HuMJngCxTs7pt65-zSUIYImpsoO8bJ_QC_pyfPMu_2O7Lh65uDvVXrkhKqOakI070vKuEK3UNnlk8m"  alt="img" style= { { zoom:"20%" } } />



| 파라미터  | 타입    | 설명                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| contractAddr   | address | for BNB, the value must be 0x0000000000000000000000000000000000000000 |
| recipient      | address | decode bech32 address, starting with `0x` . To transfer to hex string. This is a online too to decode bech32: https://slowli.github.io/bech32-buffer/ |
| amount         | uint256 | The BNB decimals on  BSC is 18. If you want to transfer one BNB, then the value should be 1e18. Besides, the value must be N * 1e10 |
| expireTime     | uint256 | 초 단위의 타임스탬프                              |

The value here should follow this equation:

```
txValue = (amount + RelayFee)/1e18
```

`RelayFee` should be 0.01BNB and it can be updated by on-chain governance. For example, if you transfer 1BNB from BSC to BC, the value should be at least 1.01BNB.

After all the above parameters have been set to proper values, users can click the transact button to build transactions, and metamask plugin will be ejected. Then users can click the confirm button in metamask to sign and broadcast transactions.


### batchTransferOutBNB

Call **batchTransferOutBNB** of TokenHub contract in MyEtherWallet:

<img src="https://github.com/binance-chain/docs-site/raw/master/docs/assets/batchTransferOutBNB.png" alt="img" style= { { zoom:"20%" } } />

| 파라미터  | 타입    | 설명                                                                    |
| -------------- | --------- | ------------------------------------------------------------ |
| recipientAddrs | address[] | decode bech32 address  to hex string. This is a online too to decode bech32: https://slowli.github.io/bech32-buffer/0 |
| amounts        | uint256[] | amount for each  recipient, should be N * 1e10               |
| refundAddrs    | address[] | sender can specify  some address as the refund address if the cross chain transfer is failed. |
| expireTime     | uint256   | 초 단위의 타임스탬프                                 |


The value here should follow this equation:

```
txValue = (sumOfAmounts + RelayFee * batchSize)/1e18
```

## BEP2를 BSC로 전송
ABC-A64 토큰을 BSC로 전송하기 위해 아래의 명령어를 실행하세요:
```bash
## mainnet
bnbcli bridge transfer-out --to 0xEe9546E92e6876EdF6a234eFFbD72d75360d91f0 --expire-time 1597543193 --chain-id Binance-Chain-Tigris --from owner --amount 10000000000:ABC-A64 --node http://dataseed4.binance.org:80

## testnet
tbnbcli bridge transfer-out --to 0xEe9546E92e6876EdF6a234eFFbD72d75360d91f0 --expire-time 1597543193 --chain-id Binance-Chain-Ganges --from owner --amount 10000000000:ABC-A64 --node http://data-seed-pre-0-s3.binance.org:80
```
## BEP20를 BC로 전송
**transferOut**나 **batchTransferOut**를 호출하기 전, 사용자들은 **approve** 메서드를 호출하여 TokenHub 컨트랙트에 충분한 잔액(allowance)을 제공해야 합니다. **transferOut** 메서드의 경우 이 allowance가 전송 액수와 같아야 합니다. **batchTransferOut**의 경우, allowance가 amount 배열의 합과 같아야 합니다.

### transferOut

<img src="https://lh3.googleusercontent.com/q8-nnt12h8gvYyMe6iwLalwzY-1jHfQ11BsSyIz3qkQPCjp_-D-dIzPxZ-HuMJngCxTs7pt65-zSUIYImpsoO8bJ_QC_pyfPMu_2O7Lh65uDvVXrkhKqOakI070vKuEK3UNnlk8m" alt="img" style= { { zoom:"25%" } } />

| 파라미터  | 타입    | 설명                                                                   |
| ------------   | ------- | ------------------------------------------------------------ |
| contractAddr   | address | BEP20 컨트랙트 주소                                     |
| recipient      | address | decode bech32 address  to hex string. This is a online too to decode bech32: https://slowli.github.io/bech32-buffer/ |
| amount         | uint256 | BEP20 token amount.  Here the decimals is 18, so the amount must be N * 1e10. |
| expireTime     | uint256 | 초 단위의 타임스탬프                            |

The value here should be RelayFee.

### 민팅

If both the BEP20 token and bep2 token are mintable, then token owners can still mint their tokens even after token binding. Besides, token owners need to ensure the total supply and the locked amount on both chains are still matched, otherwise, users might can’t transfer their tokens to another chain.

#### BC에서 토큰 민팅하게

1. Execute the following command to mint 10000 ABC-A64:
```bash
## mainnet
bnbcli token mint --symbol ABC-A64 --amount 1000000000000 --from owner --chain-id Binance-Chain-Tigris --node http://dataseed4.binance.org:80

## testnet
tbnbcli token mint --symbol ABC-A64 --amount 1000000000000 --from owner --chain-id Binance-Chain-Ganges --node http://data-seed-pre-0-s3.binance.org:80
```

2. Mint token on BSC and lock the new minted token:
* Call **mint** method of BEP20 contract, the mint amount should be 1e22.
* Transfer all minted ABC token to tokenHub contract: `0x0000000000000000000000000000000000001004`

#### BSC에서 토큰 민팅하기

1. Call **mint** of BEP20 contract to mint 10000 ABC, the mint amount should be 1e22(18 decimals).
2. Mint token on BC and lock the new minted token:

* Execute the following command to mint 10000 ABC-A64:
```bash
## mainnet
bnbcli token mint --symbol ABC-A64 --amount 1000000000000 --from owner --chain-id Binance-Chain-Tigris --node http://dataseed4.binance.org:80

## testnet
tbnbcli token mint --symbol ABC-A64 --amount 1000000000000 --from owner --chain-id Binance-Chain-Ganges --node http://data-seed-pre-0-s3.binance.org:80
```
* Transfer all minted ABC-A64 token to the pure-code-controlled address: `tbnb1v8vkkymvhe2sf7gd2092ujc6hweta38xnc4wpr`(mainnet address: `bnb1v8vkkymvhe2sf7gd2092ujc6hweta38xadu2pj`)
