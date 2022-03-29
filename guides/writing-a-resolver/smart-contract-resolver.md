# Smart contract resolver

Here is how a simple smart contract resolver looks like. This resolver is responsible for checking the `lastExecuted`state of a simple counter in another smart contract. And if 5 minutes have passed since the last execution, this resolver will prompt Gelato to execute.

{% code title="CounterResolver.sol " %}
```solidity
contract CounterResolver is IResolver {

    address public immutable COUNTER;

    constructor(address _counter) {
        COUNTER = _counter;
    }

    function checker()
        external
        view
        override
        returns (bool canExec, bytes memory execPayload)
    {
        uint256 lastExecuted = ICounter(COUNTER).lastExecuted();

        // solhint-disable not-rely-on-time
        canExec = (block.timestamp - lastExecuted) > 300;

        execPayload = abi.encodeWithSelector(
            ICounter.increaseCount.selector,
            uint256(100)
        );
    }
}
```
{% endcode %}

**A resolver should always return 2 things:**

1. bool `canExec` : wether Gelato should execute the task.
2. bytes `execPayload` :  data that executors should use for the execution.

You can find the code for `Counter` and `CounterResolver` [here](https://github.com/gelatodigital/ops/tree/master/contracts/examples/withTreasury).

**Resolvers can also accept arguments.** This is useful as you can potentially "re-use" your resolver when creating multiple tasks.&#x20;

Using the same example as above:

```solidity
    function checker(address counterAddress)
        external
        view
        override
        returns (bool canExec, bytes memory execPayload)
    {
        uint256 lastExecuted = ICounter(counterAddress).lastExecuted();

        // solhint-disable not-rely-on-time
        canExec = (block.timestamp - lastExecuted) > 300;

        execPayload = abi.encodeWithSelector(
            ICounter.increaseCount.selector,
            uint256(100)
        );
    }
```

Instead of a hardcoded `COUNTER` address, you can pass `counterAddress` as an argument.&#x20;

## Common patterns and best practices

### PokeMeReady

PokeMeReady is purely for security purposes which restricts the function call to only Gelato. This can prevent flash loan attacks. The Gelato contract already does a check if executors are EOA.

```solidity
   modifier onlyPokeMe() {
        require(msg.sender == pokeMe, "Only PokeMe");
        _;
    }
```

Note: This step is optional.

### Checking over an array

Let's say you want to automate your yield compounding for multiple pools. To avoid creating multiple tasks and having different resolvers, you could keep a list of the pools and iterate through each of the pools to see if they should be compounded. Take a look at this example.

```solidity
function checker()
	external
	view
	returns (bool canExec, bytes memory execPayload)
{
	uint256 delay = harvester.delay();
	
	for (uint256 i = 0; i < vaults.length(); i++) {
		IVault vault = IVault(getVault(i));

		canExec = block.timestamp >= vault.lastDistribution().add(delay);

		execPayload = abi.encodeWithSelector(
			IHarvester.harvestVault.selector,
			address(vault)
		);

		if (canExec) break;
	}
}
```

This resolver will return true when a certain time has elapsed since the last distribution of a pool, together with the payload to compound that specific pool.&#x20;

### Limit the Gas Price of your execution

On Ethereum Mainnet, gas will get expensive at certain times. If what you are automating is not time-sensitive and don't mind having your transaction mined later, you can limit the gas price used in your execution in your resolver.

```solidity
function checker()
	external
	view
	returns (bool canExec, bytes memory execPayload)
{
	// condition here
	
	if(tx.gasprice > 80 gwei) canExec = false;
}

```

This way, Gelato will not execute your transaction if the gas price is higher than 80 GWEI.&#x20;
