
# Claim Collateral or Rent: 

{% code title="src/handle-claim.tsx" %}
```javascript
// Make sure to add point 6's mentioned data of setup-sdk section.

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