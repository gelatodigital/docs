# Fee payment options

### Gelato Balance

Paying transaction fees with the Gelato Balance is the easiest. Simply deposit some tokens of the network you're on. Every time an execution occurs, your Gelato Balance will be used to pay for the gas and Gelato fees.&#x20;



### Transaction pays itself

You can also choose not to pre-deposit funds into your Gelato Account and have your executions pay for themselves.&#x20;

This can be done by inheriting **** [**PokeMeReady**](https://github.com/gelatodigital/poke-me/blob/master/contracts/ExampleWithoutTreasury/PokeMeReady.sol)**.**&#x20;

{% code title="PokeMeReady.sol" %}
```solidity
import {
    SafeERC20,
    IERC20
} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

interface IPokeMe {
    function gelato() external view returns (address payable);
}

abstract contract PokeMeReady {
    address public immutable pokeMe;
    address payable public immutable gelato;
    address public constant ETH = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;

    constructor(address _pokeMe) {
        pokeMe = _pokeMe;
        gelato = IPokeMe(_pokeMe).gelato();
    }

    modifier onlyPokeMe() {
        require(msg.sender == pokeMe, "PokeMeReady: onlyPokeMe");
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
    function increaseCount(uint256 amount) external onlyPokeMe {
        require(
            ((block.timestamp - lastExecuted) > 180),
            "Counter: increaseCount: Time not elapsed"
        );
        
        uint256 fee;
        address feeToken;

        (fee, feeToken) = IPokeMe(pokeMe).getFeeDetails();

        _transfer(fee, feeToken);

        count += amount;
        lastExecuted = block.timestamp;

    }
```

In the `increaseCount` function, we use `_transfer` inherited from `PokeMeReady` to pay Gelato.

`_transfer` has two parameters, `fee` and `feeToken` which has to be queried from the `PokeMe` contract by using `getFeeDetails()`

`feeToken` is set when creating your task on the Gelato Ops UI.
