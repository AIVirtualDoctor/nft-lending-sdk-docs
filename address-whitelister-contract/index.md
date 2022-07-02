# What is The Address Whitelister Contract?

TODO: ...


## Address Whitelister Contract

{% hint style="info" %}
Address Whitelister Contract must be used only my the project owner or team.
{% endhint %}

Over here, this contract is providing some great methods to get better features.
Let's first look into its main method as below  

### 1. Create address witelister contract instance
```javascript
import { useCallback, useContext } from "react";
import { ContractType, NetworkConfig, GetNetworkDetailsByChainId } from "@nftsafe/sdk";

export const useAddressWhitelister = () => {

  const signer = getSigner(); // Get signer from your created WEB3 instance or provider 
  const chainId = getChainId();  // Get currently connected chain's Id from your created WEB3 instance or provider
  const sdkNetworkConfigsByChainid = GetNetworkDetailsByChainId(Number(chainId)); // Note: ChainId must be supported chainId
  const addressWhitelisterContractAddress = sdkNetworkConfigsByChainid.addressWhitelisterContractAddresses;
  const addressWhitelisterContractInstance = new ethers.Contract(addressWhitelisterContractAddress, AddressWhitelisterAbi, signer)

  // Check if user address is alredy whitelisted or not
  const checkUserWhitelisted = useCallback(
    async (address: string) => {
      if (!addressWhitelisterContractInstance) return;
      return await addressWhitelisterContractInstance.isAddressWhitelisted(address);
    },
    [chainId,addressWhitelisterContractInstance]
  );

  // Add user address into whitelist or remove user address from whitelist
  // 1. Add user Into whitelist:  isAddingIntoWhitelist=true 
  // 2. Remove user from whitelist:  isAddingIntoWhitelist=false 
  const addUserIntoWhitelist = useCallback(
    async (address: string, isAddingIntoWhitelist: boolean, name: string) => {
      if (!addressWhitelisterContractInstance) return;
      return await addressWhitelisterContractInstance.updateWhitelistDetails(address, isAddingIntoWhitelist, name);
    },
    [chainId,addressWhitelisterContractInstance]
  );
  return { checkUserWhitelisted, addUserIntoWhitelist };
};

```
{% endcode %}


### 2. address witelister contract instance in action

```javascript
import { useAddressWhitelister } from "../hooks/useAddressWhitelister";

const addressWhitelisterContract = useAddressWhitelister();

// Check user address is in the whitelist
const isRenterWhitelisted = await checkUserWhitelisted.checkUserWhitelisted(address)

// Add user address into the whitelist
 const result = await checkUserWhitelisted.addUserIntoWhitelist(address, true , name);
// Remove user address into the whitelist
 const result = await checkUserWhitelisted.addUserIntoWhitelist(address, false, name);
```
{% endcode %}