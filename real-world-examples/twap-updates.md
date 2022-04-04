# TWAP Updates

Gelato Ops can also be used for updating Uniswap V2 TWAP oracles.&#x20;

If you are in need of a TWAP oracle, Gelato has abstracted the process of having to deploy a TWAP oracle smart contract and writing a script to update the oracle.&#x20;

Here is how you can set it up easily and how to query it.

## TWAP Oracle set up

![](<../.gitbook/assets/Screenshot 2022-04-04 at 10.32.56 AM.png>)

Head over to the [Gelato Ops UI](https://app.gelato.network) and click on Create Task.

### 1. Input Execution Address

![](<../.gitbook/assets/Screenshot 2022-04-04 at 9.35.45 AM (1).png>)

Depending on the network you are on, input the Gelato Twap Oracle address which can be found [here](../resources/contract-addresses.md).&#x20;

* Select the function `createAndUpdateOracle.`
* Input the address of the Uni v2 token pair.

### 2. Select update interval

![](<../.gitbook/assets/Screenshot 2022-04-04 at 9.35.58 AM.png>)

* Choose how often you would like to sample for your TWAP. (e.g. every 5 minutes / 1 hour / 1 day)

### 3. Create Task

![](<../.gitbook/assets/Screenshot 2022-04-04 at 9.36.46 AM.png>)

* Select "Gelato Balance" as a fee payment method and deposit some funds.
* Give your task a name and create your task!

## Querying TWAP oracle

The smart contract code can be found [here](https://github.com/gelatodigital/ops-twap-oracle/blob/master/contracts/TwapOracle.sol).

&#x20;To query the oracle, use `consult(address, address, uint)` which takes in arguments:

* `address pair` : address of Uni v2 token pair
* `address token` : token which price will be queried
* `uint amount` : the amount of token

![](<../.gitbook/assets/Screenshot 2022-04-04 at 9.59.51 AM.png>)



