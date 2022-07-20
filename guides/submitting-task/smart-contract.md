# Smart Contract

Tasks can also be created through a smart contract.

Calling `startTask()` would create a task on Gelato Ops.

```solidity
interface IOps {
    function createTask(
        address _execAddress,
        bytes4 _execSelector,
        address _resolverAddress,
        bytes calldata _resolverData
    ) public returns (bytes32 task);
}

contract Counter is OpsReady {
    uint256 public count;
    uint256 public lastExecuted;

    constructor(address payable _ops) OpsReady(_ops) {}
    
    function startTask() external {
        IOps.createTask(
            address(this), 
            this.increaseCount.selector,
            address(this),
            abi.encodeWithSelector(this.checker.selector)
        );
    }
    
    function increaseCount(uint256 amount) external onlyOps {
        require(
            ((block.timestamp - lastExecuted) > 180),
            "Counter: increaseCount: Time not elapsed"
        );

        count += amount;
        lastExecuted = block.timestamp;
    }
    
    function checker()
        external
        view
        returns (bool canExec, bytes memory execPayload)
    {
        canExec = (block.timestamp - lastExecuted) > 180;

        execPayload = abi.encodeWithSelector(
            this.increaseCount.selector,
            uint256(100)
        );
    }
}
```

When creating a task where payment is [deferred to execution time](../fee-payment-options.md#transaction-pays-for-itself) use `createTaskNoPrepayment` instead of `createTask`

```solidity
/// @notice Create a task that tells Gelato to monitor and execute transactions on specific contracts
/// @dev Requires no funds to be added in Task Treasury, assumes tasks sends fee to Gelato directly
/// @param _execAddress On which contract should Gelato execute the transactions
/// @param _execSelector Which function Gelato should eecute on the _execAddress
/// @param _resolverAddress On which contract should Gelato check when to execute the tx
/// @param _resolverData Which data should be used to check on the Resolver when to execute the tx
/// @param _feeToken Which token to use as fee payment
function createTaskNoPrepayment(
    address _execAddress,
    bytes4 _execSelector,
    address _resolverAddress,
    bytes calldata _resolverData,
    address _feeToken
) public returns (bytes32 task)
```


