# NFTSafe Subgraphs

Something about subgraphs

We can use the Moralis or any other subgraph but wait.
Good to know, even though we are providing some of the blockchains to fetch users all ERC721 and ERC1155 NFTs.

Let's take a look that how to import and use it.


```javascript
import { TypeNetworkDetails } from "@nftsafe/sdk";

// It will provide you below all the details for all sdk supported chains
type TypeNetworkDetails =
{
    chainid: {
        ....
        subgraphs: {
            collateralized: string,
            collateralFree: string,
            e721: string, // Testnet or Moralis unsupported chains
            e1155: string // Testnet or Moralis unsupported chains
        },
        moralisDetails: { isSupported: boolean, lookupValue: string },
        ....
    }
}
```
{% endcode %}

For more information about the Moralis SDK you can find it here: https://docs.moralis.io/


### Import and use subgraphs

```javascript
import { NetworkConfig, GetNetworkDetailsByChainId } from "@nftsafe/sdk";
const sdkNetworkConfigs= NetworkConfig;
const sdkNetworkConfigsByChainid = GetNetworkDetailsByChainId(Number(chainId)); // Note: ChainId must be supported chainId


// For instance,
const polygonContactDetails = GetNetworkDetailsByChainId(137)
// Or 
const polygonContactDetails = NetworkConfig[137]    

// Start using it.
// - Below code will return specified chain's subgraph endpoints
polygonContactDetails.subgraphs.collateralized
polygonContactDetails.subgraphs.collateralFree
// Only if available in SDK for given chain.
polygonContactDetails.subgraphs.e721  
polygonContactDetails.subgraphs.e1155 

```
{% endcode %}


