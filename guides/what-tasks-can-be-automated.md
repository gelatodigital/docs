# What tasks can be automated

Functions that can be automated should follow these properties:

* They need to be functions which are usually called by the development team or external keepers, not "user facing" functions called by users directly
* They need to be either `public` or `external`
* They do not have access restrictions like an `onlyOwner` modifier, unless [`PokeMe.sol`](../resources/contract-addresses.md#gelato-ops) is a whitelisted address
* They do not require `msg.sender` to be `tx.origin`, as the `msg.sender` of your contract function will be `PokeMe.sol`
