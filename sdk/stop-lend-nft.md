# NFT Stop Lending 

{% hint style="info" %}
Make sure to add point 6's mentioned data of setup-sdk section.
{% endhint %}

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

