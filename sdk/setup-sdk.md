# Intialize and use Collateralized or CollateralFree NFTSafe instance

Import NFTSafe class in your project and call **`NFTSafe`** Constructor by passing required parameter as shown below
```javascript
// Get NFtSafe Collateralized and CollateralFree Contract by ContractType

// import NFTSafe, ContractType from the SDK 
import {NFTSafe, ContractType } from "@nftsafe/sdk";
//The "ContractType" enum which is already present in SDK will help you to get the specific type of contract.
//************ Collateralized Contract Instance **************
new NFTSafe(signer, Number(chainId), ContractType.COLLATERALIZED);

//or For Collateral Free 
//************ Collateral Free Contract Instance ************** 
new NFTSafe(signer, Number(chainId), ContractType.COLLATERAL_FREE);
```
{% endcode %}


{% hint style="info" %}
**Good to do:** Craete NFTSafe react hook so it will give you strong grip for using sdk over more pages.
{% endhint %}

So, let's create react hook of NFtSafe contract object for flexible use of it.

{% code title="src/useNFTSafeSdk.tsx" %}
```javascript
import { useCallback, useContext, useMemo } from "react";
import { GetNetworkDetailsByChainId, NFTSafe, ContractType, ALL_SUPPORTED_CHAIN_IDS, DEFAULT_CHAIN_NAME } from "@nftsafe/sdk";

export const useNFTSafeSDK = ({isCollateralized, signer, chainId }:{isCollateralized:boolean,  signer:Signer, chainId:SupportedChainIds }): NFTSafe | undefined => {

  // Get all details like contract addresses, chain details, moralis sdk details, and other supportive utils by chainid
  var sdkConfig;
  const nftSafe = useMemo(() => {
    if (!signer || !chainId) return;
    // ALL_SUPPORTED_CHAIN_IDS method avaiable in sdk help you to check it out that connected chain is supported or not with NFTSafe SDK
    if(!ALL_SUPPORTED_CHAIN_IDS.includes(Number(chainId))){
        return;
    }else{
        sdkConfig = GetNetworkDetailsByChainId(Number(chainId));
        if(sdkConfig!.isSupported){
            return;
        }
    }

    if (isCollateralized) {
        /// This Will return Collateralized Contract Instance
      return new NFTSafe(signer, Number(chainId), ContractType.COLLATERALIZED);
    }else{
         /// This Will return Collateral Free Contract Instance
      return new NFTSafe(signer, Number(chainId), ContractType.COLLATERAL_FREE);
    }
  }, [isCollateralized,signer,chainId]);

  return nftSafe;
};

```
{% endcode %}


### Use NFTSafe Contract methods and create necessary contract objects

Now that the SDK setup is done successfully we can use the power of NFTSafe and other contracts along with supportive utils.

Let's first, get our desired contract instance

```javascript
// import supportive types
import { PaymentToken, ContractType, NFTStandard } from "@nftsafe/sdk";
import { BigNumber } from "ethers";
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
Make sure to add this above data into following function as lend, rent, stop lend, stop rent and claim collateral and rent.
{% endhint %}

<!-- 
### 5. LEND NFt

{% code title="src/index.tsx" %}
```javascript
// import supportive types
import { PaymentToken, ContractType, NFTStandard } from "@nftsafe/sdk";
// use 'nftSafeContractInstance' selected NftSafe contract instance to LEND Nft 

//************** Check and Approve NFT *****************

const handleCheckApprove = async(lendingInput, isERC721, isCollateralized) => {
  if (!lendingInput) return EMPTY;
  // paymentToken will be type of "PaymentToken" instance of "@nftsafe/sdk" 
  // It's value will be as paymentToken set by Lender  
  var nftTypeContractInstance;

  // Get valid type of NFT type contract object  
  if (isERC721) {
    nftTypeContractInstance= new ethers.Contract(lendingInput.nftAddress,ERC721Abi,signer);  
  } else {
    nftTypeContractInstance= new ethers.Contract(lendingInput.nftAddress,ERC1155Abi,signer);
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
const handleApproveAll = async(rentingInput) => {
  if (!lendingInput) return EMPTY;
  // paymentToken will be type of "PaymentToken" instance of "@nftsafe/sdk" 
  // It's value will be as paymentToken set by Lender   
 var nftTypeContractInstance;

// Get valid type of NFT type contract object 
  if (isERC721) {
    nftTypeContractInstance= new ethers.Contract(lendingInput.nftAddress,ERC721Abi,signer);  
  } else {
    nftTypeContractInstance= new ethers.Contract(lendingInput.nftAddress,ERC1155Abi,signer);
  }
  // set for which type contract we want to check approval
  const nftSafeContractAddress=isCollateralized ? collateralizedContractAddress : collateralFreeContractAddresses;
  return await nftTypeContractInstance.setApprovalForAll(nftSafeContractAddress, true);
};

/// **************** End NFT approve  *************  


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





### 5. RENT NFt

{% code title="src/index.tsx" %}
```javascript
// import supportive types
import { PaymentToken, ContractType, NFTStandard, NFTSafeAbi } from "@nftsafe/sdk";
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


/*
If user is first time and as above check approve is not efficient then call this function and 
start giving approvel of payment token to the contract address
 */ 
const startApproveAll = async(rentingInput) => {
  if (!paymentTokenProviderInstance) return EMPTY;
  // paymentToken will be type of "PaymentToken" instance of "@nftsafe/sdk" 
  // It's value will be as paymentToken set by Lender   
  const paymentTokenAddress =await  paymentTokenProviderInstance.getPaymentOption(rentingInput.paymentToken);
  const erc20= new ethers.Contract(paymentTokenAddress, ERC20Abi, signer);
  // set contract for which we are approving
    const nftSafeContractAddress=isCollateralized ? collateralizedContractAddress : collateralFreeContractAddresses;
  return await erc20.approve(nftSafeContractAddress, MAX_UINT256);
};

/// **************** End payment token approve  *************  

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


### 5. Stop LEND NFt

{% code title="src/index.tsx" %}
```javascript
// import supportive types
import { PaymentToken, ContractType, NFTStandard } from "@nftsafe/sdk";

const handleStopLending = (stopLendingInputs:{stopLendingInputs:[]}) => {
    if (!nftSafeContractInstance) return EMPTY;
    
    const nftStandards: NFTStandard[] = [];
    const nftAddresses: string[] = [];
    const tokenIds: BigNumber[] = [];
    const lendingIds: BigNumber[] = [];

    stopLendingInputs.forEach((item) => {
      nftStandards.push((item.isERC721 ? NFTStandard.E721 : NFTStandard.E1155));
      nftAddresses.push(item.address);
      tokenIds.push(BigNumber.from(item.tokenId));
      lendingIds.push(BigNumber.from(item.lendingId));
    });

    nftSafeContractInstance.stopLending(nftStandards, nftAddresses, tokenIds, lendingIds);
};
```
{% endcode %}



### 5. Stop RENT NFt



{% code title="src/index.tsx" %}
```javascript


//************** Check and Approve of NFT *****************

const handleCheckApprove = async(lendingInput, isERC721, isCollateralized) => {
  if (!lendingInput) return EMPTY;
  // paymentToken will be type of "PaymentToken" instance of "@nftsafe/sdk" 
  // It's value will be as paymentToken set by Lender  
  var nftTypeContractInstance;

  // Get valid type of NFT type contract object  
  if (isERC721) {
    nftTypeContractInstance= new ethers.Contract(lendingInput.nftAddress,ERC721Abi,signer);  
  } else {
    nftTypeContractInstance= new ethers.Contract(lendingInput.nftAddress,ERC1155Abi,signer);
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
const handleApproveAll = async(rentingInput) => {
  if (!lendingInput) return EMPTY;
  // paymentToken will be type of "PaymentToken" instance of "@nftsafe/sdk" 
  // It's value will be as paymentToken set by Lender   
 var nftTypeContractInstance;

// Get valid type of NFT type contract object 
  if (isERC721) {
    nftTypeContractInstance= new ethers.Contract(lendingInput.nftAddress,ERC721Abi,signer);  
  } else {
    nftTypeContractInstance= new ethers.Contract(lendingInput.nftAddress,ERC1155Abi,signer);
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



### 5. Claim Collateral or Rent: 

{% code title="src/index.tsx" %}
```javascript
// import supportive types
import { PaymentToken, ContractType, NFTStandard } from "@nftsafe/sdk";

const handleClaimRentOrCollateral = (selectedItems:{selectedItems:[]}) => {
    if (!nftSafeContractInstance) return EMPTY;
    
    const nftStandards: NFTStandard[] = [];
    const nftAddresses: string[] = [];
    const tokenIds: BigNumber[] = [];
    const lendingIds: BigNumber[] = [];
    const rentingIds: BigNumber[] = [];

    selectedItems.forEach((item) => {
      nftStandards.push((item.isERC721 ? NFTStandard.E721 : NFTStandard.E1155));
      nftAddresses.push(item.address);
      tokenIds.push(BigNumber.from(item.tokenId));
      lendingIds.push(BigNumber.from(item.lendingId));
      rentingIds.push(BigNumber.from(item.rentingId));
    });

    nftSafeContractInstance.claimRentOrCollateral(nftStandards, nftAddresses, tokenIds, lendingIds, rentingIds);
};
```
{% endcode %} -->


Now, time use the power of NFTSafe Sdk by using follwing basic function.