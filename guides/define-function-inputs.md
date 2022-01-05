# Define function inputs

If the function you are automating accepts arguments, there are two ways you can define the arguments.&#x20;

### Pre-define inputs (No resolver)

Pre defining the function arguments would mean that every time Gelato calls the function, it would be using the same argument.&#x20;

### Dynamic inputs via Resolver

By using a resolver, you can dynamically encode the arguments of the function you are automating.&#x20;

Here is an example.&#x20;

This is the function we are automating. `increaseCount` increases a counter on the smart contract by `amount` which is the argument.

```solidity
    function increaseCount(uint256 amount) external onlyPokeMe {
        require(
            ((block.timestamp - lastExecuted) > 300),
            "Counter: increaseCount: Time not elapsed"
        );

        count += amount;
        lastExecuted = block.timestamp;
    }
```

This resolver returns the data to the function call `increaseCount`.

```solidity
    function checker()
        external
        view
        override
        returns (bool canExec, bytes memory execPayload)
    {
        uint256 lastExecuted = ICounter(COUNTER).lastExecuted();

        canExec = (block.timestamp - lastExecuted) > 300;
        
        uint256 countToIncrease = ICounter(COUNTER).count * 2

        execPayload = abi.encodeWithSelector(
            ICounter.increaseCount.selector,
            countToIncrease
        );
    }

```

The increment on each execution is different every time as `countToIncrease` is different after every execution.&#x20;
