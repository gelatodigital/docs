# What tasks can be automated

Functions that can be automated should follow these properties:

* Public / external function
* Does not have `onlyOwner` modifier unless `PokeMe` is the owner
* Does not require `msg.sender` to be `tx.origin`
