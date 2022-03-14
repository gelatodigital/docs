# Fee payment options

### Gelato Balance

Paying transaction fees with the Gelato Balance is the easiest. Simply deposit some tokens of the network you're on. Every time an execution occurs, your Gelato Balance will be used to pay for the gas and Gelato fees.

### Transaction pays itself

You can also choose not to pre-deposit funds into your Gelato Account and have your executions pay for themselves.

This can be done by inheriting [**O**psReady](https://github.com/gelatodigital/ops/blob/master/contracts/Gelato/OpsReady.sol)**.**

{% code title="OpsReady.sol" %}
```solidity
import {
    SafeERC20,
    IERC20
} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

interface IOps {
    function gelato() external view returns (address payable);
}

abstract contract OpsReady {
    address public immutable ops;
    address payable public immutable gelato;
    address public constant ETH = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;

    constructor(address _ops) {
        ops = _ops;
        gelato = IOps(_ops).gelato();
    }

    modifier onlyOps() {
        require(msg.sender == ops, "OpsReady: onlyOps");
        _;
    }

    function _transfer(uint256 _amount, address _paymentToken) internal {
        if (_paymentToken == ETH) {
            (bool success, ) = gelato.call{value: _amount}("");
            require(success, "_transfer: ETH transfer failed");
        } else {
            SafeERC20.safeTransfer(IERC20(_paymentToken), gelato, _amount);
        }
    }
}
```
{% endcode %}

The function which will be automated needs to have the ability to pay Gelato whenever executors call it. Below is an example of this function.

```solidity
    function increaseCount(uint256 amount) external onlyOps {
        require(
            ((block.timestamp - lastExecuted) > 180),
            "Counter: increaseCount: Time not elapsed"
        );
        
        uint256 fee;
        address feeToken;

        (fee, feeToken) = IOps(ops).getFeeDetails();

        _transfer(fee, feeToken);

        count += amount;
        lastExecuted = block.timestamp;

    }
```

In the `increaseCount` function, we use `_transfer` inherited from `OpsReady` to pay Gelato.

`_transfer` has two parameters, `fee` and `feeToken` which has to be queried from the `Ops` contract by using `getFeeDetails()`

`feeToken` is set when creating your task on the Gelato Ops UI.
