# NFT Stop Renting 


{% code title="src/handle-stop-rent.tsx" %}
```javascript
// import supportive types
import { PaymentToken, ContractType, NFTStandard } from "@nftsafe/sdk";
import { ethers,BigNumber } from "ethers";
import { ERC20Abi, ERC721Abi,ERC1155Abi } from "./abi";
// import network config data like 
import {collateralizedContractAddress, collateralFreeContractAddresses, paymentTokenProviderContractAddress, ...} from '/blockchainConfig';


const signer = getSigner(); // Get signer from your created WEB3 instance or provider 
var nftSafeContractInstance;
var paymentTokenProviderInstance;

// useNFtSafe hook to get desired NftSafe contract instance 
if(isCollateralized) {
  nftSafeContractInstance = useNFTSafeSDK(isCollateralized, signer, chainId); // isCollateralized = true
}else{
  nftSafeContractInstance= useNFTSafeSDK(isCollateralized, signer, chainId); // isCollateralized = false
}

 paymentTokenProviderInstance=  new ethers.Contract(paymentTokenProviderContractAddress, NFTSafeAbi, signer)

```
{% endcode %}

{% hint style="info" %}
Make sure to add this above data into following function.
{% endhint %}

### 1.Check approve nft
{% code title="src/handle-stop-rent.tsx" %}
```javascript

import {  ERC721Abi,ERC1155Abi } from "./abi";
export const MAX_UINT256 =
  "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff";

const handleCheckApprove = async(stopRentingInput, isERC721, isCollateralized) => {
  if (!stopRentingInput) return EMPTY;
  // paymentToken will be type of "PaymentToken" instance of "@nftsafe/sdk" 
  // It's value will be as paymentToken set by Lender  
  var nftTypeContractInstance;

  // Get valid type of NFT type contract object  
  if (isERC721) {
    nftTypeContractInstance= new ethers.Contract(stopRentingInput.nftAddress,ERC721Abi,signer);  
  } else {
    nftTypeContractInstance= new ethers.Contract(stopRentingInput.nftAddress,ERC1155Abi,signer);
  }
  // set for which type contract we want to check approval
  const nftSafeContractAddress=isCollateralized ? collateralizedContractAddress : collateralFreeContractAddresses;
  const isApproved =await nftTypeContractInstance.isApprovedForAll(currentUserAddress, nftSafeContractAddress);
  return isApproved;
};
```
{% endcode %}

### 2.Approve nft
{% code title="src/handle-stop-rent.tsx" %}
```javascript
/*
If user is first time and as above check approve is not efficient then call this function and 
start giving approvel of payment token to the contract address
 */ 
const handleApproveAll = async(stopRentingInput) => {
  if (!stopRentingInput) return EMPTY;
  // paymentToken will be type of "PaymentToken" instance of "@nftsafe/sdk" 
  // It's value will be as paymentToken set by Lender   
 var nftTypeContractInstance;

// Get valid type of NFT type contract object 
  if (isERC721) {
    nftTypeContractInstance= new ethers.Contract(stopRentingInput.nftAddress,ERC721Abi,signer);  
  } else {
    nftTypeContractInstance= new ethers.Contract(stopRentingInput.nftAddress,ERC1155Abi,signer);
  }
  // set for which type contract we want to check approval
  const nftSafeContractAddress=isCollateralized ? collateralizedContractAddress : collateralFreeContractAddresses;
  return await nftTypeContractInstance.setApprovalForAll(nftSafeContractAddress, true);
};

/// **************** End NFT approve  *************  
```
{% endcode %}


### 3.Start stop nft renting

{% code title="src/handle-stop-rent.tsx" %}
```javascript
// import supportive types
import { PaymentToken, ContractType, NFTStandard } from "@nftsafe/sdk";

const handleStopRenting = (rentingInputs:{rentingInputs:[]}) => {
    if (!nftSafeContractInstance) return EMPTY;
    
    const nftStandards: NFTStandard[] = [];
    const nftAddresses: string[] = [];
    const tokenIds: BigNumber[] = [];
    const lendingIds: BigNumber[] = [];
    const rentingIds: BigNumber[] = [];

    rentingInputs.forEach((item) => {
      nftStandards.push((item.isERC721 ? NFTStandard.E721 : NFTStandard.E1155));
      nftAddresses.push(item.address);
      tokenIds.push(BigNumber.from(item.tokenId));
      lendingIds.push(BigNumber.from(item.lendingId));
      rentingIds.push(BigNumber.from(item.rentingId));
    });

    nftSafeContractInstance.stopRenting(nftStandards, nftAddresses, tokenIds, lendingIds, rentingIds);
};
```
{% endcode %}