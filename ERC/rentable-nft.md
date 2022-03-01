# Renting Extension For ERC-1155 Standart

## Simple Summary

An extension of the ERC-1155 standard to enable ERC1155 NFT tokens to be rentable 

## Abstract

This extension defines some extra functions to the ERC-1155 standart for renting NFT tokens to another account.
This helps NFT owners to rent their assets to someone for some period of time. 
Because an ERC-1155 contract can have fungible tokens (FT) and non-fungible tokens (NFT), this functionality will be usable by only NFTs.
It's significantly important for the gaming industry where such NFTs might have additional abilities that affect the gaming process.


## Specification

This functionality will be usable for only NFT tokens. This requires that the contract should implement the split id functionality. 
So if the most significant bit of the token id is 1 then that token is NFT.

```solidity
pragma solidity ^0.8.0;

/**
    @title ERC-1155 Rentable
 */
interface ERC1155 /* is ERC165 */ {
    /**
        @dev This event will be emitted when token is rented
        tokenId: token id to be rented
        owner:  principal owner address
        renter: renter address
        expiresAt:  end of renting period as timestamp
    */
    event Rented(uint256 indexed tokenId, address indexed lord, address indexed renter, uint256 expiresAt);

    /**
        @dev This event will be emitted when renting is finished by the owner or renter
        tokenId: token id to be rented
        owner:  principal owner address
        renter: renter address
        expiresAt:  end of renting period as timestamp
    */
    event FinishedRent(uint256 indexed tokenId, address indexed lord, address indexed renter, uint256 expiresAt);

    /**
        @notice rentOut
        @dev rent a token to another address
        @param renter: renter address
        @param tokenId: token id to be rented
        @param expiresAt: end of renting period as timestamp 
    */
    function rentOut(address renter, uint256 tokenId, uint256 expiresAt) external;

    /**
        @notice finishRenting
        @dev  Renter can run this anytime but owner can run after expire time.
        @param tokenId: token id
    */
    function finishRenting(uint256 tokenId) external;

    /**
        @notice principalOwner
        @dev  get the actual ownert of the rented token
        @param tokenId: token id
    */
    function principalOwner(uint256 tokenId) external returns (address);

    /**
        @notice isRented
        @dev  get whether or not the token is rented
        @param tokenId: token id
    */
    function isRented(uint256 tokenId) external returns (bool);
}

