# NFt Renting


{% code title="src/handle-rent.tsx" %}
```javascript
// import supportive types
import { PaymentToken, ContractType, NFTStandard } from "@nftsafe/sdk";
import { ethers,BigNumber } from "ethers";
import { ERC20Abi, ERC721Abi,ERC1155Abi } from "./abi";
// import network config data like 
import {collateralizedContractAddress, collateralFreeContractAddresses, paymentTokenProviderContractAddress, ...} from '/blockchainConfig';

export const MAX_UINT256 =
  "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff";
const signer = getSigner(); // Get a signer from your created WEB3 instance or provider 
var nftSafeContractInstance;
var paymentTokenProviderInstance;

// Use the NFTSafe hook to get desired contract instance
if(isCollateralized) {
  nftSafeContractInstance = useNFTSafeSDK(isCollateralized, signer, chainId); // isCollateralized = true
}else{
  nftSafeContractInstance= useNFTSafeSDK(isCollateralized, signer, chainId); // isCollateralized = false
}

 paymentTokenProviderInstance=  new ethers.Contract(paymentTokenProviderContractAddress, NFTSafeAbi, signer)

```
{% endcode %}

{% hint style="info" %}
Make sure to add the above data into the following functions such as to lend, rent, stop lending, stop renting, and claim collateral or rent.

{% endhint %}


### 1.Check payment token approve
{% code title="src/handle-rent.tsx" %}
```javascript

const startCheckApprove = async(rentingInput, isCollateralized) => {
  if (!paymentTokenProviderInstance) return EMPTY;
  // paymentToken will be type of "PaymentToken" instance of "@nftsafe/sdk" 
  // It's value will be as paymentToken set by Lender   
  const paymentTokenAddress =await  paymentTokenProviderInstance.getPaymentOption(rentingInput.paymentToken);
  const erc20= new ethers.Contract(paymentTokenAddress, ERC20Abi, signer);
  // set for which type contract we want to check approval
  const nftSafeContractAddress=isCollateralized ? collateralizedContractAddress : collateralFreeContractAddresses;
  const allowence =  await erc20.allowance(currentUserAddress,nftSafeContractAddress);
  return allowance.lt(BigNumber.from(MAX_UINT256).div(2));
};

```
{% endcode %}

### 2.Approve payment token
{% code title="src/handle-rent.tsx" %}
```javascript
/*
If user is first time and as above check approve is not efficient then call this function and 
start giving approvel of payment token to the contract address
 */ 
const startApproveAll = async(rentingInput,isCollateralized) => {
  if (!paymentTokenProviderInstance) return EMPTY;
  // paymentToken will be type of "PaymentToken" instance of "@nftsafe/sdk" 
  // It's value will be as paymentToken set by Lender   
  const paymentTokenAddress =await  paymentTokenProviderInstance.getPaymentOption(rentingInput.paymentToken);
  const erc20= new ethers.Contract(paymentTokenAddress, ERC20Abi, signer);
  // set contract for which we are approving
    const nftSafeContractAddress=isCollateralized ? collateralizedContractAddress : collateralFreeContractAddresses;
  return await erc20.approve(nftSafeContractAddress, MAX_UINT256);
};
```
{% endcode %}

### 3.Start nft renting
{% code title="src/handle-rent.tsx" %}
```javascript
const handleRent = (rentingInputs:{rentingInputs:[]}) => {
    if (!nftSafeContractInstance) return EMPTY;
    
    const nftStandards: NFTStandard[] = [];
    const nftAddresses: string[] = [];
    const tokenIds: BigNumber[] = [];
    const lendingIds: BigNumber[] = [];
    const rentDurations: number[] = [];
    const rentAmounts: number[] = [];

    lendingInputs.forEach((item) => {
      nftStandards.push((item.isERC721 ? NFTStandard.E721 : NFTStandard.E1155));
      nftAddresses.push(item.address);
      tokenIds.push(BigNumber.from(item.tokenId));
      lendingIds.push(BigNumber.from(item.lendingId));
      rentDurations.push(Number(convertToSecond(item.rentDuration)));
      rentAmounts.push(Number(nft.rentAmount));
    });

    nftSafeContractInstance.rent(nftStandards, nftAddresses, tokenIds, lendingIds, rentDurations, rentAmounts);
};
```
{% endcode %}
