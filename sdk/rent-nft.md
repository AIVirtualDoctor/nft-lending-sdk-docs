# NFt Renting

{% hint style="info" %}
Make sure to add point 6's mentioned data of setup-sdk section.
{% endhint %}

{% code title="src/handle-rent.tsx" %}
```javascript
import {  ERC20Abi } from "./abi";
import { BigNumber } from "ethers";
export const MAX_UINT256 =
  "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff";

//************** Check and Approve payment token *****************

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
