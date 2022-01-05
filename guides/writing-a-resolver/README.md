# Writing a resolver

### **What is a resolver and when do you need it?**

A resolver is a smart contract in which developers can embed their logic of "when" an execution should occur and what data should executors use for the execution. Gelato will query these resolvers periodically to check if it is time to execute.&#x20;

You **need** a resolver if

* You want to define the conditions of "when" Gelato should execute your task.
* You want to have different arguments for the function you are automating on each execution.

You **do not** need a resolver if

* You simply want to repeatedly call a smart contract function with the same or without arguments (e.g. every 1 hour).&#x20;
* If you just want Gelato to try to execute a particular function in case it does NOT revert

We give developers the freedom to choose what kind of resolver they want to use.&#x20;

Make sure to go through [overview.md](../../getting-started/overview.md "mention") before continuing.



### Smart contract resolver

A smart contract resolver is deployed on the corresponding network. (e.g. Ethereum Mainnet, Fantom) If you feel more comfortable writing in solidity, or you have a simple condition which only uses on-chain data, you could go with this option.



### Polywrap resolver (coming soon)

A [Polywrap](https://polywrap.io/#/) resolver is hosted on IPFS. If your condition requires doing HTTP requests or subgraph queries, this would be the way to go.



