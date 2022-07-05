# Explore SDK utils


<!-- ### Get Contract and Network details:
Get all details like contract addresses, chain details, moralis sdk details, and other supportive utils according chainid

{% code title="src/blockchainConfig.tsx" %}
```javascript
//import as above mentioned all exported modules in your in page 
import { SupportedChainIds, ContractType,NetworkConfig, GetNetworkDetailsByChainId, CONTRACT_TYPE_LIST,SupportedChainIds, ALL_SUPPORTED_CHAIN_IDS ....} from '@nftsafe/sdk'
//--or--
// import module specifically like  NFTSafe, ContractType from the SDK 
import { NetworkConfig, GetNetworkDetailsByChainId } from "@nftsafe/sdk";
```
{% endcode %} -->


### 1.Network Config
SDK provides the "NetworkConfig" JSON to get the Contract addresses, Subgraph URL, Chain information, and the Blockchain supporting utils as follows.
```javascript

import { NetworkConfig, GetNetworkDetailsByChainId } from "@nftsafe/sdk";
// NetworkConfig will provide you with below all the details for all SDK-supported chains.

type TypeNetworkDetails =
{
    chainid: {
        name: string,
        chainId: number,
        shortName: string,
        chain: string,
        networkId: number,
        nativeCurrency: {},
        rpc: string[],
        faucets: string[],
        explorers: { name: string, url: string, standard: string },
        infoURL: string,
        logoURL: string,
        collateralizedContractAddresses: string[],
        collateralFreeContractAddresses: string[],
        paymentTokenProviderContractAddresses: string[],
        addressWhitelisterContractAddresses: string[],
        e721ContractAddresses: string[], // Testnet
        e721BContractAddresses: string[], // Testnet
        e1155ContractAddresses: string[], // Testnet
        e1155BContractAddresses: string[], // Testnet
        wETHContractAddresses: string[],
        daiContractAddresses: string[],
        usdcContractAddresses: string[],
        usdtContractAddresses: string[],
        tUSDContractAddresses: string[],
        utilsContractAddresses: string[],
        subgraphs: {
            collateralized: string,
            collateralFree: string,
            e721: string, // Testnet or Moralis unsupported chains
            e1155: string // Testnet or Moralis unsupported chains
        },
        moralisDetails: { isSupported: boolean, lookupValue: string },
        chainApiId: string, 
        isSupported: boolean,
        isTestnet: boolean
    }
}


const sdkNetworkConfigs= NetworkConfig;
const sdkNetworkConfigsByChainid = GetNetworkDetailsByChainId(Number(chainId)); // Note: ChainId must be supported chainId

// Now, you can use all parameter as below
console.log(sdkNetworkConfigsByChainid.rpc); // Network RPC

/*NOTE: Contract address will be in List format 
- Contract Addreess List containe latest address at first position and so on.
- Contract address array version priority [V n...3,2,1] (Latest contract at zero index)
*/
// Return List of all version contract addresses
console.log(sdkNetworkConfigsByChainid.collateralizedContractAddresses); //Collateralized Contract Addresses : format [] 
console.log(sdkNetworkConfigsByChainid.collateralFreeContractAddresses); //Collateral Free Contract Addresses : format []
console.log(sdkNetworkConfigsByChainid.paymentTokenProviderContractAddresses); //Payment Token Provide rContract Address : format []

//- Get the latest contract address as follows.
export const collateralizedContractAddress=sdkNetworkConfigsByChainid.collateralizedContractAddresses[0];
console.log(collateralizedContractAddress); //Collateralized Latest Contract Address

export const collateralFreeContractAddresses=sdkNetworkConfigsByChainid.collateralFreeContractAddresses[0];
console.log(collateralFreeContractAddresses); //Collateral Latest Free Contract Address

export const paymentTokenProviderContractAddress=sdkNetworkConfigsByChainid.paymentTokenProviderContractAddresses[0];
console.log(paymentTokenProviderContractAddress); //Payment Token Provider Latest Contract Address


// ...
console.log(sdkNetworkConfigsByChainid.rpc.toString());
console.log(sdkNetworkConfigsByChainid.subgraphs.toString());
// ...


/* 
- For instance,

    const polygonContactDetails = GetNetworkDetailsByChainId(137)
          // Or 
    const polygonContactDetails = NetworkConfig[137]   
    
    Get Contract Addresses:
    polygonContactDetails.collateralizedContractAddresses[0]
    polygonContactDetails.collateralFreeContractAddresses[0]
    ....
    Get subgraph endpoint:
    polygonContactDetails.subgraphs.collateralized
    polygonContactDetails.subgraphs.collateralFree
    ....
    Get Other details:
    polygonContactDetails.name
    polygonContactDetails.explorers.url
  ....
*/

```
{% endcode %}


### 2.Supported Payment Tokens:
```javascript
import { PaymentToken } from "@nftsafe/sdk";
// You can use it as follows.
// Ex.
PaymentToken.WETH
PaymentToken.DAI
PaymentToken.USDC
//...
```
{% endcode %}


### 3.Supported NFT Standard:
```javascript
import { NFTStandard } from "@nftsafe/sdk";
// You can use it as below.
// Ex.
NFTStandard.E721
NFTStandard.E1155
```
{% endcode %}



### 4.Supported ChainIds:
```javascript
import { SupportedChainIds } from "@nftsafe/sdk";
// You can use it as follows.
// Ex.
SupportedChainIds.POLYGON_MAINNET
SupportedChainIds.POLYGON_TESTNET
SupportedChainIds.ETHEREUM_KOVAN
//...
```
{% endcode %}



### 5.Contract Type:
```javascript
import { ContractType } from "@nftsafe/sdk";
// You can use it as below
// Ex.
ContractType.COLLATERALIZED
ContractType.COLLATERAL_FREE
ContractType.PAYMENT_TOKEN_PROVIDER
// ...
```
{% endcode %}




{% hint style="info" %}
Create constant or object of network config because we are going to use the above addresses and other utils in lend, rent, stop lending, stop renting, and claim collateral and rent functions.
{% endhint %}



## Tip of the iceberg

As you can probably already see NFTSafe is a true superpower for blockchain developers. But this small demo is just the tip of the iceberg. NFTSafe provides endless tools and features for any blockchain use-case. Most importantly, <mark style="color:green;">**everything is cross-chain by default**</mark>.


{% hint style="info" %}

{% endhint %}