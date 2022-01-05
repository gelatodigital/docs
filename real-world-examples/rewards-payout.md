# Rewards payout

## Unicly

[Unicly](https://www.unic.ly) uses Gelato Ops for daily rewards generation for stakers.

Gelato calls this `addRewards` in their [staking contract ](https://etherscan.io/address/0x2e30b5fa7bfa8d51a4668284c763af112e664031#code)recurringly once everyday.&#x20;

```solidity
function addRewards(address rewardToken, uint256 amount) override external poolExists(rewardToken) {
        require(amount > 0, "UnicStaking: Amount must be greater than zero");
        IERC20Upgradeable(rewardToken).safeTransferFrom(msg.sender, address(this), amount);
        RewardPool storage pool = pools[rewardToken];
        pool.totalRewardAmount = pool.totalRewardAmount.add(amount);
        emit AddRewards(rewardToken, amount);
}
```

Since the condition to call this function is time-dependent, writing a resolver is not required. &#x20;

You can create a task through the [Gelato Ops](https://beta.app.gelato.network) UI directly.

![](<../.gitbook/assets/Screenshot 2021-11-25 at 10.32.55 AM.png>)

On the task creation page, select the Time option for "When" and input your desired interval between executions.&#x20;
