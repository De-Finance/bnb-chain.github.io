---
sidebar_label: Using Black IDE
hide_table_of_contents: false
sidebar_position: 2
---

# Black IDE를 사용하여 BSC에 NFT 배포하기

이 튜토리얼에서는 Black IDE를 사용하여 BNB 스마트 체인(BSC) 테스트넷에 NFT(Non-funcible Token)(ERC721/1155)를 발행하는 방법에 대한 단계별 가이드를 독자에게 제공합니다. 이 문서는 BSC 테스트넷에서 NFT를 발행 및 양도하는 방법을 자세히 안내합니다. 이 튜토리얼에 사용되는 기술 스택에는 솔리디티, Truffle, MetaMask 및 Black IDE가 포함됩니다.

## 학습 요점:
이 튜토리얼은 다음 학습 요점에 대한 지식을 얻는 데 도움이 됩니다.
- 스마트 컨트랙트 개발을 위한 Black IDE
- 키쌍을 관리하고 Black IDE의 계정에 BNB 토큰을 전송하기
- BSC 테스트넷에 메타마스크 연결입니다.
- 스마트 컨트랙트 개발
- NFT를 발행, 민팅 및 양도

## 기술 스택 세부
-	BlackIDE v0.15.4
-	Truffle v5.5.19 (core: 5.5.19)
-	MetaMask Wallet v10.16.1
-	Docker v20.10.14

## 간단한 기술 스택 소개
1. **솔리디티:** 가장 인기 있는 객체 지향 고급 스마트 계약 프로그래밍 언어 중 하나입니다. 솔리디티에 대한 자세한 내용은 여기를 참조하십시오.
2. **메타마스크 지갑 브라우저 익스텐션:** 메타마스크 크롬 확장을 사용하는 것이 좋습니다. 이것은 크롬 브라우저를 유효한 블록체인 네트워크에 연결할 수 있는 웹 지갑입니다.
3. **Black IDE:** Black IDE는 통합 개발 환경(IDE)으로, EVM 호환 스마트 계약을 더 빠르고 쉽게 개발할 수 있습니다. Black IDE는 데스크톱 및 웹(Black IDE Web) 응용 프로그램을 모두 제공합니다.

## 환경 설정하기
이 튜토리얼은 가능한 한 단순하게 유지하는 것이 목표이므로 리소스를 최대한 적게 사용합니다. 다음과 같은 도구를 사용했습니다.
* 메타마스크 지갑
* 브라우저에서 메타마스크 지갑 익스텐션이 설치되어 실행 중인지 확인하십시오.
* BSC 테스트넷과 함께 사용할 수 있도록 메타마스크 지갑을 구성합니다. 다음 세부 정보를 사용하여 BSC 테스트넷을 추가합니다. 자세한 내용은 여기를 참조하십시오.
* 네트워크 이름: BSC 테스트넷입니다.
* RPC URL: https://data-seed-prebsc-1-s1.binance.org:8545/
* 체인 ID: 97
* 통화 기호: BNB
* 블록 탐색기 URL: https://testnet.bscscan.com 
* Black IDE: 데스크톱 앱과 웹 앱을 모두 사용할 수 있으며 사용자의 편의에 따라 선택할 수 있습니다. 웹 앱은 OpenZeppelin 컨트랙트를 가져오기를 지원하지 않기 때문에 이 튜토리얼에서는 데스크톱 앱을 사용했습니다. 
* BlackIDE에 필요한 디펜던시를 다운로드/설치합니다.

![image](https://user-images.githubusercontent.com/93580180/177942609-e2c942a6-342c-46cd-b794-92fc8e72bdc0.png)

## Black IDE에 로그인하기
1. Black IDE 데스크톱 애플리케이션을 엽니다. BSC 테스트넷에서 NFT에 대한 스마트 컨트랙트를 컴파일하고 배포하는 데 사용할 것입니다.
2. 로그인 버튼을 클릭하고 GitHub 계정을 사용하여 인증합니다.


## 새 프로젝트 생성하기
3. 프로젝트 옆에 있는 New 버튼을 클릭하여 새 프로젝트를 만듭니다. 

4. 장치에서 프로젝트를 저장할 위치(예: "BSC-NFT")를 지정하고 드롭다운 목록에서 프로젝트 유형을 "Basics - ERC20, ERC721, & ERC1155(v31+)"로 선택합니다. 그런 다음 Create 버튼을 클릭하여 프로젝트를 생성합니다.


5. 이 튜토리얼의 스마트 컨트랙트는 샘플일 뿐이며 언제든지 수정하고 개선할 수 있습니다.

## 스마트 컨트랙트 생성하기
6. 컨트랙트 메뉴를 확장하고 기본 파일을 삭제합니다. 


7. 컨트랙트 메뉴를 마우스 오른쪽 버튼으로 클릭하고 새 파일을 선택합니다. 파일 이름(예: BSC-NFT.sol)을 지정한 다음 만들기 단추를 클릭합니다.


## 스마트 컨트랙트 코드 작성
8. 다음 코드를 스마트 컨트랙트 파일에 복사합니다. 우리는 이[repo](https://github.com/RumeelHussainbnb/ERC721_NFT/blob/main/BSC-NFT.sol)의 계약 코드를 사용했습니다.
9. 필요에 따라 토큰의 ```MINT_PRICE```, ```MAX_SUPPLY```, ```name```, ```symbol```을 변경하세요. 또한 토큰에 따라 ```_baseURI``를 변경해야 합니다.
10.	
![image](https://user-images.githubusercontent.com/93580180/177949895-a095fdb5-f770-4530-84f6-8854a0d7a5eb.png)

## 기본 프로젝트 설정 편집
10. config.json 파일을 클릭하여 기본 설정을 변경합니다. 우리의 경우 메인 파일 이름을 컨트랙트 이름인 BSC-NFT.sol로 변경합니다. 마찬가지로 배포할 스마트 컨트랙트 이름(이 경우 BSCNFT.json)을 변경하십시오. 


 
## Black IDE를 BSC 테스트넷에 연결하기
11. Black IDE를 BSC 테스트넷에 연결하려면 오른쪽 상단 모서리에 있는 네트워크 메뉴에서 드롭다운 아이콘을 클릭한 다음 BNB 체인 레이블에서 테스트넷을 선택합니다.

![image](https://user-images.githubusercontent.com/93580180/177948186-e052e522-7069-4072-abae-fd0e6c819ee6.png)

12.	Click on the ![image](https://user-images.githubusercontent.com/93580180/177943789-3557fde5-8805-4b03-ace8-05d2ace216c0.png) icon in the bottom left corner of the IDE to generate new keypair to perform transactions. You can skip this step if you already have generated a keypair. On the Keypair Manager, click on the CREATE button to generate new keypair.

![image](https://user-images.githubusercontent.com/93580180/177944146-eb6e2f1e-95f0-4b00-8458-c8145b008d15.png)

13.	Specify your desired name for the keypair, in our case BSC-Testnet-Key. Then click on the CREATE button. Remember to keep your private keys securely and not share them with anyone.

![image](https://user-images.githubusercontent.com/93580180/177944170-fa9ed3bc-53d9-41f3-8a46-a07f56fee1d7.png)

## Acquire BNB Test Tokens
*	Initially, the balance of a newly created key pair is 0.0 ETH. To get BNB test tokens, you can use the [BSC Testnet Faucet](https://testnet.binance.org/faucet-smart/).
*	Copy your public address from the keypair manager

![image](https://user-images.githubusercontent.com/93580180/177944290-d06f2f06-e256-4110-8936-809c0f78e0fa.png)

*	Paste this on the facet and acquire test tokens as required, as shown below. A green pop-up is displayed on the successful transfer of test tokens.

![image](https://user-images.githubusercontent.com/93580180/177944333-ca8aefed-fec2-4271-aa3e-d2ccc301eb6c.png)
 
*	 Close and re-open keypair manager to verify that the balance has been updated. Wait for approx. 1-2 mins for balance to get updated.

![image](https://user-images.githubusercontent.com/93580180/177944370-3aa70613-be45-4558-8c83-aca1a00557c3.png)

## Deploy Smart Contract on BSC Testnet
1.	Select the appropriate Solidity compiler version from the bottom right corner of the IDE, Solc (0.8.4), ![image](https://user-images.githubusercontent.com/93580180/177944415-e733562a-54ad-4ed8-85a5-f17c79edfeac.png)
in our case. 
2.	Click on the Build icon ![image](https://user-images.githubusercontent.com/93580180/177944483-ff523eed-017d-4265-b722-78ded06fe826.png) to build your smart contract. Upon successful build, the project navigation pane reflects a new folder named build. This folder contains contracts folder that has json files of the contracts built. All of the contracts imported in our BSCNFT contract are also built and imported as json files. 

![image](https://user-images.githubusercontent.com/93580180/177948331-ece850ae-01fd-479b-b25f-d1f28c3400f6.png)

3.	After successfully building your contract, it’s time to deploy the contract. Click on the Deploy icon ![image](https://user-images.githubusercontent.com/93580180/177944540-10d86198-03f2-40d8-8ac0-c013483c6458.png) for deploying your smart contract. Specify the details for your contract, as shown below, then click on the Estimate & Deploy button. The wizard will auto-estimate and fill the gas limit for your contract. Then click the Deploy button. 

![image](https://user-images.githubusercontent.com/93580180/177944618-47b0fb1d-0ce3-4512-9b29-66f7a3416325.png)
![image](https://user-images.githubusercontent.com/93580180/177944632-839f7ac5-c9d5-4d80-b6fe-0b6be2e6c3fb.png)

4.	Deployment details will pop-up, as shown in the figure below.

![image](https://user-images.githubusercontent.com/93580180/177944726-41c20038-1fc5-434a-b61f-be54f36f3ac6.png)

5.	The status of the transaction will be updated to confirmed after the transaction is confirmed as shown in the figure below

![image](https://user-images.githubusercontent.com/93580180/177944768-dc622464-832f-4afd-8ccd-0529c675d46d.png)

6.	You can also view this transaction by clicking on the transaction icon in the bottom left on the IDE.

![image](https://user-images.githubusercontent.com/93580180/177944786-8b7f3e34-e562-406b-890e-dcc22d48313f.png)

## Interact with deployed smart contract and Mint NFTs
1.	You can also interact with the contract using the different functions. Click on the Transactions Icon on the bottom-left corner of the IDE and then transaction of deployment of your smart contract. On the transaction details, click on the contract address to access the functions to interact with the smart contract. The left most column has all the Write Functions. The middle column has the View Functions and the right most column has the Events details.

![image](https://user-images.githubusercontent.com/93580180/177948835-197860e5-8f25-4692-bc34-467a997f98a1.png)
 
## Mint NFTs
1.	As per our smart contract, when the contract is deployed, unless the NFTs are minted they won’t be visible in the wallet. 
2.	Create another keypair as defined previously. We will be issuing i.e. minting NFTs to the public address of this new keypair.
3.	To mint i.e. issue an NFT to a specific user we use the “safeMint” function of the deployed smart contract. As shown in Steps 1 and 2 in the figure below, navigate to the deployed contracts, then in the left-most column click the drop-down menu to view the list of write functions available for use with the deployed contract. Select the “safeMint” function. 
4.	Use the safeMint function to mint new NFTs to a specific user address. As shown in figure above, steps 3 to 6, enter the “ETH to send” as the minting price of NFT, as per our smart contract the minting price is 50000000000000000 Wei, i.e., 0.05 ETH. We entered 0.06 ETH to cover the transaction charges as well. Then select the address to whom you want to issue (mint) an NFT to. Here, for the To address use the newly generated keypair in the section above. After this, click the transact button  to execute the safeMint function. For the Signer, ensure that you are using the account that was used to deploy the smart contract. 

![image](https://user-images.githubusercontent.com/93580180/177949113-3de5d538-852f-4a21-aa75-47d0231b6521.png)

5.	To confirm what transfers have occurred, execute the Transfer event from the right most column. This will display the list of NFT transfers along with NFT token id, as shown in the figure below.

![image](https://user-images.githubusercontent.com/93580180/177945187-4e426ba2-a63f-4648-a259-fc9506ab5cb1.png)

6.	To confirm owner of an NFT, use the ownerOf function. Pass the token id as input to the function, as illustrated in the figure below.

![image](https://user-images.githubusercontent.com/93580180/177945228-3a984146-dfa5-4a7b-9306-6258e9990f2a.png)

## View Your NFTs in Metamask Wallet
1.	On the receiving end, the user can import the NFT token details into their Metamask wallet to view the assets. Please note that currently, Metamask Web Extension does not support the use of NFTs however, the mobile app version does support it. For the next steps to view the owned NFTs in your Metamask wallet, we will be using the Metamask Mobile Application.
2.	Open Keypair manager on the Black IDE and copy the private key of the keypair that you minted i.e., transfer NFT.

![image](https://user-images.githubusercontent.com/93580180/177945281-060b95ce-2912-49a2-aa5d-bbf848ba9688.png)

3.	On the Metamask wallet mobile app, import an account using this key pair. Enter the private key copied in the previous step and click import.

![image](https://user-images.githubusercontent.com/93580180/177954799-b86dae87-5274-4408-9d0b-5b52682549d1.png)

4.	After importing account, the next step is to add the BSC Testnet configuration to the wallet. Ensure that you are using the same account whose pubic address was issued the NFT.

![image](https://user-images.githubusercontent.com/93580180/177950571-4674930b-c8c6-4480-8808-ea587af2bb2d.png)

5.	Ensure that your account is connected to the BSC Testnet. Also, ensure that you have enough BNB test tokens in your account. If not, you can use the BSC Testnet Faucet to acquire some, as mentioned earlier.

![image](https://user-images.githubusercontent.com/93580180/177945438-84033dff-6d51-4fe6-875b-3b12bfe815c1.png)
 
6.	To view the owned NFT assets your Metamask Mobile Wallet, click on the NFTs tab and then on the Import Tokens. Fill in the NFT details. In the address field, pass the address of the deployed contract and in the Id field pass the tokenID. Then click the Import button.

![image](https://user-images.githubusercontent.com/93580180/177949365-52efd22c-25ac-4eac-b47e-82349d6b0a5c.png)

## Conclusion 
In this tutorial, we provided a step-to-step guide on how to issue, mint and transfer NFTs on the BSC Testnet using the BlackIDE from Obsidian Labs.. The technology stack used in this tutorial includes Solidity, Truffle, MetaMask, and BlackIDE. Check out our [GitHub](https://github.com/bnb-chain/bnb-chain-tutorial) for more tutorials on how to develop on BSC. If you have any questions or are stuck, reach out to us on our [Discord Channel](https://discord.com/channels/789402563035660308/912296662834241597).

