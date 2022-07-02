# NFt Lending

{% hint style="info" %}
Make sure to add point 6's mentioned data of setup-sdk section.
{% endhint %}

### Check NFT Approve

{% code title="src/handle-lend.tsx" %}
```javascript

import {  ERC721Abi,ERC1155Abi } from "./abi";

const handleCheckApprove = async(lendingInput,  isCollateralized) => {
  if (!lendingInput) return EMPTY;
  // paymentToken will be type of "PaymentToken" instance of "@nftsafe/sdk" 
  // It's value will be as paymentToken set by Lender  
  var nftTypeContractInstance;

  // Get valid type of NFT type contract object  
  if (isERC721.isERC721) {
    nftTypeContractInstance= new ethers.Contract(lendingInput.nftAddress,ERC721Abi,signer);  
  } else {
    nftTypeContractInstance= new ethers.Contract(lendingInput.nftAddress,ERC1155Abi,signer);
  }
  // set for which type contract we want to check approval
  const nftSafeContractAddress=isCollateralized ? collateralizedContractAddress : collateralFreeContractAddresses;
  const isApproved =await nftTypeContractInstance.isApprovedForAll(currentUserAddress, nftSafeContractAddress);
  return isApproved;
};

```
{% endcode %}


### Approve NFT

{% code title="src/handle-lend.tsx" %}
```javascript

import {  ERC721Abi,ERC1155Abi } from "./abi";

/*
If user is first time and as above check approve is not efficient then call this function and 
start giving approvel of payment token to the contract address
 */ 
const handleApproveAll = async(rentingInput,isCollateralized) => {
  if (!lendingInput) return EMPTY;
  // paymentToken will be type of "PaymentToken" instance of "@nftsafe/sdk" 
  // It's value will be as paymentToken set by Lender   
 var nftTypeContractInstance;

// Get valid type of NFT type contract object 
  if (rentingInput.isERC721) {
    nftTypeContractInstance= new ethers.Contract(lendingInput.nftAddress,ERC721Abi,signer);  
  } else {
    nftTypeContractInstance= new ethers.Contract(lendingInput.nftAddress,ERC1155Abi,signer);
  }
  // set for which type contract we want to check approval
  const nftSafeContractAddress=isCollateralized ? collateralizedContractAddress : collateralFreeContractAddresses;
  return await nftTypeContractInstance.setApprovalForAll(nftSafeContractAddress, true);
};

```
{% endcode %}


### Start Lend NFT

{% code title="src/handle-lend.tsx" %}
```javascript

const handleLend = (lendingInputs:{lendingInputs:[]}) => {
    if (!nftSafeContractInstance) return EMPTY;

    const nftStandards: NFTStandard[] = [];
    const nftAddresses: string[] = [];
    const tokenIds: BigNumber[] = [];
    const lendAmounts: number[] = [];
    const maxRentDurations: number[] = [];
    const minRentDurations: number[] = [];
    const dailyRentPrices: number[] = [];
    const paymentOptions: number[] = [];
    const collateralPrices: number[] = [];

    lendingInputs.forEach((item) => {
      nftStandards.push((item.isERC721 ? NFTStandard.E721 : NFTStandard.E1155));
      nftAddresses.push(item.address);
      tokenIds.push(BigNumber.from(item.tokenId));
      lendAmounts.push(item.lendAmount);
      maxRentDurations.push(convertToSecond(item.maxDuration));
      minRentDurations.push(convertToSecond(item.minDuration));
      dailyRentPrices.push(item.borrowPrice);
      paymentOptions.push(item.paymenToken);
      collateralPrices.push(item.nftPrice);
    });

    nftSafeContractInstance.lend(
        nftStandards,
        nftAddresses,
        tokenIds,
        lendAmounts,
        maxRentDurations,
        minRentDurations,
        dailyRentPrices,
        paymentOptions,
        collateralPrices
    );
};


```
{% endcode %}

