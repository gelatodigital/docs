---
description: Using Gelato in production today
---

# Example Use Cases

### **Instadapp:** DeFi Asset Management Platform

Our most notable and long-standing integration is with [Instadapp](https://medium.com/gelato-network/instadapp-turns-on-defi-automation-powered-by-gelato-network-c964a5a61791?source=collection\_home---4------6-----------------------), one of the largest DeFi Asset Management Platforms in the world. The integration began by securing undercollateralized debt positions by refinancing them to other protocols with lower collateral requirements, saving these debt positions from liquidation. Gelato Executors work diligently in the background to monitor the health of positions and spring into action when variables change such as a crashing of ETH’s price.

### **Sorbet Finance:** Limit & Stop Loss Orders on AMM's like Uniswap

In April 2021, we debuted [Sorbet Finance](https://www.sorbet.finance), Gelato’s "feature playground" that is built on top of Uniswap and Quickswap, which automates trading actions on decentralized exchanges. These actions include what traders already enjoy on centralized counterparts such as [Limit Orders](https://www.sorbet.finance/limit-order) as well as Dollar-Cost averaging Strategies and in the future, we intend to add more features built specifically for DeFi such as Liquidity provider management and automated Uniswap v3 rebalancing. All of these functionalities involve automatically executing token swaps based on certain conditions, set by the user) that are executed once those instructions have been met.

### **G-UNI: Automated Uniswap V3 LP Positions**

Monitoring Uniswap v3 LP positions can be a very time-consuming and complex task, dependent on the user's risk profile. This would require the user to continuously monitor prices and ensure that he is constantly rebalancing his LP position to continue acquiring fees as price moves outside of his current set bounds. With [an automated rebalancing strategy](https://medium.com/gelato-network/lp-like-a-pro-in-uni-v3-auto-rebalancing-2b8f98aa81ab), the user can rest assured that not only are his acquired fees automatically reinvested, the Executors keep a close eye monitoring the price, and when the price has exited the set bounds for an extended period of time. When the price moves out of a certain range, the LP position manages the ranges between which liquidity should be provided to always provide concentrated liquidity around the current market price. The strategy also automatically reinvests earned fees back into Uniswap to achieve a compounding effect. This is done in a fully decentralized and non-custodial matter using Gelato’s network of bots.   &#x20;

The advantage of this includes that users can sit back and relax as all the difficulties that come with monitoring LP positions are taken care of. In addition, since [G-UNI](https://medium.com/gelato-network/lp-like-a-pro-in-uni-v3-auto-rebalancing-2b8f98aa81ab) mints an ERC-20 token, it has the capability to be used for liquidity mining schemes to incentivize holders to provide concentrated liquidity around certain price ranges.&#x20;

### Liquidation Protection for Debt Position on KeeperDAO

On KeeperDAO, every time collateral prices start to decline, Gelato automates the repayment user’s debt position on lending protocols, to avoid costly collateral liquidations. At the time of the native integration going live, KeeperDAO had $240 million in TVL, now secured via Gelato’s automation infrastructure.

### Gelato Ops: Web3's Multi-Chain Smart Contract Automation Hub

[Gelato Ops](https://ops.gelato.network) enables web3 developers to automate tasks via an easy-to-use interface. Instead of manually triggering functions or having to build and monitor their own bot infrastructure, developers can rely on Gelato Ops to automate any arbitrary task on most EVM-based blockchains. Gelato Ops is now live on Ethereum, Polygon, Fantom, BNB Chain, Avalanche, and Arbitrum — with more networks to come.

