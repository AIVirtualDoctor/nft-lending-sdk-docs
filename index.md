# Connnect your platform with SDK

This guide will show you how you can do it in just a few easy steps.

### 1. Creating React App

To start a new Create React App project with TypeScript, you can run:

{% tabs %}
{% tab title="npx" %}
```
npx create-react-app my-app --template typescript
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn create react-app my-app --template typescript
```
{% endtab %}
{% endtabs %}

### 2. Install the SDK

Make sure to have **WEB3**, **WEB3** or  **Wallet-connect** installed as dependencies. Then install **@nftsafe/sdk**:

{% tabs %}
{% tab title="npm" %}
```
npm install @nftsafe/sdk
```
{% endtab %}

{% tab title="yarn" %}
```
yarn add @nftsafe/sdk
```
{% endtab %}
{% endtabs %}

### 3. Import the SDK
You can import any of the function from the below  list of exported function in the sdk:

{% code title="src/index.tsx" %}
```javascript
export { NFTSafe } from './nftSafe';
export { INFTSafe, PaymentToken, NFTStandard } from './types';
export { packPrice, unpackPrice, toPaddedHex, prepareBatch, numberToByte4, numberToByte8, numberToByte16, numberToByte32, byteToNumber } from './utils';
export { TypeNetworkDetails, TypeNetworkConfig, CONTRACT_TYPE_LIST, ContractType, SupportedChainIds, ALL_SUPPORTED_CHAIN_IDS, NetworkConfig, GetNetworkDetailsByChainId } from './networkConfig';
export { AddressWhitelisterAbi } from './abi/AddressWhitelisterAbi';
export { AddressWhitelisterBytecode } from './abi/AddressWhitelisterBytecode';
export { PaymentTokenProviderAbi } from './abi/PaymentTokenProviderAbi';
export { PaymentTokenProviderBytecode } from './abi/PaymentTokenProviderBytecode';
export { NFTSafeAbi } from './abi/NFTSafeAbi';
export { NFTSafeBytecode } from './abi/NFTSafeBytecode';
export { DEFAULT_CHAINID, DEFAULT_CHAIN_NAME } from './consts';
```
{% endcode %}

Now, your can import required element and call it.
```javascript
import { NFTSafe, ... DEFAULT_CHAIN_NAME } from "@nftsafe/sdk";
```
{% endcode %}