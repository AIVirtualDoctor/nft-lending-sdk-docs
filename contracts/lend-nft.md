# NFt Lending


{% code title="src/handle-lend.tsx" %}
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
    const collateralPrices: number[] = []; // Must be Zero for collateral free 

    lendingInputs.forEach((item) => {
      nftStandards.push((item.isERC721 ? NFTStandard.E721 : NFTStandard.E1155));
      nftAddresses.push(item.address);
      tokenIds.push(BigNumber.from(item.tokenId));
      lendAmounts.push(item.lendAmount);
      maxRentDurations.push(convertToSecond(item.maxDuration));
      minRentDurations.push(convertToSecond(item.minDuration));
      dailyRentPrices.push(item.borrowPrice);
      paymentOptions.push(item.paymenToken);
      collateralPrices.push(item.nftPrice); // Must be Zero for collateral free 
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
        collateralPrices   // Must be Zero for collateral free 
    );
};


```
{% endcode %}




#### Note: Good to have below validation 
>1. the "nftStandards" should be one of the available in "NFTStandard" type of SDK.
>2. The "maxRentDurations" must be grater than "minRentDurations".
>3. The "paymentOptions" should be one of the available in "PaymentToken" type of SDK.
>4. The "collateralPrices" value should be ZERO for the CollateralFree contract.


- In addition, for Lend method only it will use two unique methods for below to values:
    * dailyRentPrices: dailyRentPrices.map(x => packPrice(Number(x))),
    * collateralPrices: collateralPrices.map(x => packPrice(Number(x))), 


#### 1. "packPrice" : Used by SDK
>- Converts a number into the format that is acceptable by the NFTSafe contract.
>- TLDR; to fit a single storage slot in the NFTSafe contract, we split the whole
>- and decimal parts of a number to only have a maximum of 4 digits. That means, the
>- maximum price is 9999.9999. If more decimals are supplied, they are truncated.
>- If the price exceeds the maximum whole part, this throws.
>- @param price value to pack
>- @returns price format that is acceptable by NFTSafe contract

#### 2. "unpackPrice" : Used in frontend
>-  when you fetch data from the Blockchain after that you need to use "unpackPrice" method to get the actual value from a formatted value
>- price is from 1 to 4294967295. i.e. from 0x00000001 to 0xffffffff
>
>    dailyRentPrice = unpackPrice(formated_dailyRentPrice),
>    collateralPrice = unpackPrice(formated_collateralPrice),



For instance: 
- User entred value:  dailyRentPrices = 6 WETH , collateralPrices = 20 WETH
- Used by SDK
>dailyRentPrices = packprice(6);      // output:   0x00060000
>collateralPrices  = packPrice(20)   // output:   0x00140000
- Used in frontend
>dailyRentPrices =unpackPrice(0x00060000);     // output:  6
>collateralPrices  = unpackPrice(0x00140000)   // output:  20

