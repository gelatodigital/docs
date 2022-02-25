# Gelato Ops UI

To submit a task, head to the [Gelato Ops](https://app.gelato.network) UI.

![](<../../.gitbook/assets/Screenshot 2021-11-07 at 12.32.48 PM.png>)

Click on New Task.

![](<../../.gitbook/assets/Screenshot 2021-11-07 at 12.33.25 PM.png>)

* Choose a name for your task. (e.g. "Harvest vault every 10 minutes")

## Execution contract details

![](<../../.gitbook/assets/Screenshot 2021-11-07 at 12.36.09 PM.png>)

* Fill in the contract address of the function you want to automate.
* Select the function you want to automate.&#x20;

![](<../../.gitbook/assets/Screenshot 2021-11-07 at 12.37.01 PM.png>)

If your function accepts arguments

* Choose how you want your arguments to be passed.&#x20;

If you are not sure about this, see [define-function-inputs.md](../define-function-inputs.md "mention")

## When to execute

### Pre-define inputs

![](<../../.gitbook/assets/Screenshot 2021-11-07 at 10.09.47 PM.png>)

With pre-defined inputs chosen, you will see two options.

**Time** - Task will be recurringly executed by the time given by the user with an interval.

**Whenever possible** - Task will be executed continuously whenever the transaction will not fail.

{% hint style="warning" %}
Make sure the function can only be executed from time to time if "Whenever possible" is selected
{% endhint %}

### Dynamic inputs via resolver

![](<../../.gitbook/assets/Screenshot 2021-11-07 at 10.18.39 PM.png>)

If you are using a resolver, fill in the resolver address that is deployed.

## Paying for fees

![](<../../.gitbook/assets/Screenshot 2021-11-07 at 10.28.25 PM.png>)

Choose how you would like to pay for the execution fees for your task.&#x20;

Deposit some tokens if "Gelato Balance" is chosen.

If you are not sure about this, see [fee-payment-options.md](../fee-payment-options.md "mention") .



## Done!

Once your task creation transaction is mined, you will be redirected to the Task Page.

![](<../../.gitbook/assets/Screenshot 2021-11-07 at 10.32.53 PM.png>)

Here you can see details and monitor your task.&#x20;
