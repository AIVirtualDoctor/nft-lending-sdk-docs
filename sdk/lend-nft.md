# NFt Lending


{% code title="src/handle-lend.tsx" %}
```javascript
// import supportive types
import { PaymentToken, ContractType, NFTStandard } from "@nftsafe/sdk";
import { BigNumber } from "ethers";
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

### 1.Check nft approve

{% code title="src/handle-lend.tsx" %}
```javascript

const handleCheckApprove = async(lendingInput,  isCollateralized) => {
  if (!lendingInput) return EMPTY;
  // paymentToken will be type of "PaymentToken" instance of "@nftsafe/sdk" 
  // It's value will be as paymentToken set by Lender  
  var nftTypeContractInstance;

  // Get valid type of NFT type contract object  
  if (lendingInput.isERC721) {
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


### 2.Approve nft

{% code title="src/handle-lend.tsx" %}
```javascript

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


### 3.Start nft lending

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

