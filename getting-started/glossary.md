# Glossary

**Execution contract** - The smart contract with the function you want to automate and which executors will be calling.

**Gelato account** - Similar to a wallet, users can deposit tokens that will be used to pay for fees. Think of it as a pre-paid SIM card or your car's gas tank which you have to fill up periodically

**A Resolver** - A smart contract that tells Gelato when it should execute your task and the data to be used for the execution. It returns a `bool` and a `bytes` payload.

**Transaction pays itself** - Gelato fee is paid from the execution itself instead of deducting from the task creator's Gelato account.
