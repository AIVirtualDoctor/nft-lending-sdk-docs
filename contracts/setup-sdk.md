# Intialize and use Collateralized or CollateralFree NFTSafe instance

Import NFTSafe class in your project and call **` NFTSafe`** Constructor by passing the required parameter as shown below
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
**Good to do:** Create NFTSafe react hook so it will give you a strong grip for using sdk over more pages.
{% endhint %}

So, let's create react hook of the NFtSafe contract object for flexible use of it.

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



Now that the SDK setup is done successfully we can use the power of NFTSafe and other contracts along with supportive utils.

It is time to use the power of NFTSafe SDK by using the following basic functions.
