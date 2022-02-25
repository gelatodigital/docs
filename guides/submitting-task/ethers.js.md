# Ethers.js

[Here](https://github.com/gelatodigital/ops/blob/687a34fe56349486234c920e1072eab18221bf36/scripts/submit-task.js) is a sample script to create a task programmatically.

```javascript
  const ops = await ethers.getContractAt("Ops", OPS, user);
  const counter = await ethers.getContractAt("Counter", COUNTER, user);
  const counterResolver = await ethers.getContractAt(
    "CounterResolver",
    RESOLVER,
    user
  );

  const selector = await ops.getSelector("increaseCount(uint256)");
  const resolverData = await ops.getSelector("checker()");

  const txn = await ops.createTask(
    counter.address,
    selector,
    counterResolver.address,
    resolverData
  );
```
