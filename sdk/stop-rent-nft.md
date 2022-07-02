# Stop RENT NFt

{% code title="src/handle-stop-rent.tsx" %}
```javascript

// Make sure to add point 6's mentioned data of setup-sdk section.
import {  ERC721Abi,ERC1155Abi } from "./abi";
export const MAX_UINT256 =
  "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff";

//************** Check and Approve of NFT *****************
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



{% code title="src/index.tsx" %}
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