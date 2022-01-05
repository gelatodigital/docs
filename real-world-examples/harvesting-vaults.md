# Harvesting vaults

## ETHA Lend

[ETHA](https://www.ethalend.org) is using Gelato Ops for automated yield harvesting.&#x20;

This is the function which Gelato will be calling. `harvestVault` claims yield generated from a pool and re-deposits them back into the pool.

```solidity
function harvestVault(IVault vault) public onlyAfterDelay(vault) {
	// Amount to Harvest
	uint256 afterFee = vault.harvest();
	require(afterFee > 0, "!Yield");

	IERC20 from = vault.rewards();
	IERC20 to = vault.target();

	address connector = getBestConnector(
		address(from),
		address(to),
		afterFee
	);

	// Quickswap path
	address[] memory path;

	if (connector == address(0)) {
		path = new address[](2);
		path[0] = address(from);
		path[1] = address(to);
	} else {
		path = new address[](3);
		path[0] = address(from);
		path[1] = connector;
		path[2] = address(to);
	}

	// Swap underlying to target
	from.approve(address(ROUTER), afterFee);
	uint256 received = ROUTER.swapExactTokensForTokens(
		afterFee,
		1,
		path,
		address(this),
		block.timestamp + 1
	)[path.length - 1];

	// Send profits to vault
	to.approve(address(vault), received);
	vault.distribute(received);

	emit Harvested(address(vault), msg.sender);
}
```

ETHA uses this resolver below to check for ready to be harvested pools.&#x20;

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



The resolver loops through an array of pools. And for each vault, if a defined `delay` has elapsed since the previous harvest time, `canExec` will return `true` , prompting Gelato to execute the task. `execPayload` will be the data to the function call `harvestVault(address vault)` and its argument is the address of the vault to be harvested.
