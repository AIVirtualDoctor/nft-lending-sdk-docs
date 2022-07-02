
# Claim Collateral or Rent: 

{% hint style="info" %}
Make sure to add point 6's mentioned data of setup-sdk section.
{% endhint %}

{% code title="src/handle-claim.tsx" %}
```javascript

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
{% endcode %}



{% hint style="success" %}
 We have  successfully connected and used NFTsafe SDk
{% endhint %}

Feel free to explore the rest of the documentation in order to grasp the full power of NFTSafe SDK.
