# NFT Stop Lending 

{% code title="src/handle-stop-lend.tsx" %}
```javascript
// import supportive types
import { PaymentToken, ContractType, NFTStandard } from "@nftsafe/sdk";
import { BigNumber } from "ethers";

const signer = getSigner(); // Get signer from your created WEB3 instance or provider 
var nftSafeContractInstance;

// useNFtSafe hook to get desired NftSafe contract instance 
if(isCollateralized) {
  nftSafeContractInstance = useNFTSafeSDK(isCollateralized, signer, chainId); // isCollateralized = true
}else{
  nftSafeContractInstance= useNFTSafeSDK(isCollateralized, signer, chainId); // isCollateralized = false
}

```
{% endcode %}

{% hint style="info" %}
Make sure to add this above data into following function.
{% endhint %}


### Start stop nft lending
{% code title="src/handle-stop-lend.tsx" %}
```javascript

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

