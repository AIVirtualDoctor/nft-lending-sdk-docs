# Why use NFTSafe SDK?

## What is NFTSafe SDK?
**NFTSafe SDK provides the full-stack workflow for building high-performance dapps. Fully compatible with your favorite web3 tools and services.** NFTSafe Collateralized and Collateral Free contract with NFT Lend, Rent, Stop Lending, Stop Renting, Claim Collateral or Rent, and other more features, subgraphs, chain, and other utils, and so much more. All features are accessed through an easy-to-use SDK. All features NFTSafe SDK provides are cross-chain by default 🤯.

## Why use NFTSafe SDK?
NFTSafe SDK is the fastest way to build and deploy dApps on Ethereum, BSC, Polygon, and others (more coming). All NFTSafe SDK dApps are cross-chain by default. Building on NFTSafe SDK ensures that your dApp is future-proof. Even if new blockchains are invented, your dApp will instantly work on any chain.

Whether you are building your first blockchain project or are already a seasoned developer - NFTSafe SDK will make your projects easier to build, maintain and improve.

The video below explains more in-depth what NFTSafe SDK is and how it helps your project.

TODO:{% embed url="" %}
A short lecture explaining the value-proposition of NFTSafe SDK.
{% endembed %}

Let's quickly summarize the different components of an NFTSafe SDK Server that you will be using.

### SDK Utils (Network and Contract configs)
Here is where all of Network and Contract configs data is being stored. such as the Contract addresses, Subgraph URL, Chain information, and other Blockchain supporting utils.

You can then use this data instantaneously in your dApp frontend.

You can read more under the sections: [NFTSafe SDK Utils](network-config/explore-network-config.md).

### Contracts
NFTSafe provides you with Collateralized and Collateral Free contracts with NFT Lend, Rent, Stop Lending, Stop Renting, Claim Collateral or Rent, and other features, subgraphs, chain, and other utils.

It also gives you a Payment Token Provider Contract which will help you to set and fetch Payment Tokens, NFTSafe  Collateralized, and Collateral Free contracts will accept only that payment tokens. 

There is one more important contract out of all contracts which is the Address Whitelister Contract provided by SDK. you will get to know more about it later on this page. 

You can read more about this in the [Contract section](contracts/index.md).

### The NFTSafe SDK
NFTSafe' SDK is how we tie all of this together. Our JavaScript SDK is how your dApp interacts with your NFTSafe SDK. Using the SDK, you can display user's all NFT,and perform lend, rent, stop lending, stop renting or Claim Collateral or Rent operation on any NFT along with display user that current and archive data in your dApp frontend.

You can read more about the SDK by[ clicking here](contracts/setup-sdk.md).


### Address Whitelister Contract
NFTSafe SDK's Address Whitelister Contract give you the power to check user address is whitelisted or not and, add or remove user address into a whitelist.
Note: it should only be used by Admin.

You can read more about the Address Whitelister Contract by[ clicking here](address-whitelister-contract/index.md).


### The Subgraphs
NFTSafe SDK's give you the deployed subgraph endpoints which all you can direactly use by simple making request with passing query with desired parameter. It will give you data of all user's lended, rented, Stop Lended, Stop Rented and Claim Collateral or Rent NFT by NFTSafe Contracts and you can show that in your dApp frontend.

You can read more about the Subgraphs by[ clicking here](subgraphs/index.md).


## Welcome to the NFTSafe SDK Documentation
TODO:{% embed url="" %}

We're excited for you to discover all the cool things you can build using NFTSafe SDK with just a few lines of code. Before we get started, let's go over some important information.

## Expectations
* The docs assume that you have some programming knowledge.
* The docs are a work in progress and receive regular updates.
* If you find something confusing in the docs or have suggestions for improvements let us know by posting in the [NFTSafe SDK forum](https://forum.NFTSafe SDK.io) or by contacting community .
<!-- * If you find a bug in the NFTSafe SDK SDK or server dashboard, please report it in [this GitHub repo](https://github.com/NFTSafe SDKWeb3/issue-tracker), along with a detailed description and steps to reproduce the issue. For technical questions, or if you need help with your code, then please create a post in the [NFTSafe SDK forum.](https://forum.NFTSafe SDK.io) If you're unsure where to post, then please ask by creating a post in the [NFTSafe SDK forum](https://forum.NFTSafe SDK.io) or on the [FAQ & How to Get Help](https://forum.NFTSafe SDK.io/c/faq/12) page. -->

## Prerequisites
The documentation assumes that you have some type of knowledge with JavaScript, working with objects, querying databases, and some Web3 development.

If you're new to Web3 development, then please watch the following video:

TODO:{% embed url="" %}

For more experienced developers, then make sure to watch the following video on how to build a dApp with NFTSafe SDK on supported chain.

TODO:{% embed url="" %}

## Setup Your First dApp with NFTSafe SDK
See the [Getting Started](/getting-started.md)section to guide you through setting up your first server with NFTSafe SDK and how to integrate it with your dApp:

{% content-ref url="getting-started/" %}
[getting-started](getting-started.md)
{% endcontent-ref %}


{% hint style="info" %}
**Good to know:**
{% endhint %}
