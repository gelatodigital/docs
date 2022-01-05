# Polywrap resolver

Before we start, it is best to checkout Polywrap's [documentation](https://docs.polywrap.io/getting-started/what-is-polywrap) to get some insight into Polywrap.

{% hint style="warning" %}
Polywrap resolver is still in Beta!
{% endhint %}

Begin by cloning this [Polywrap resolver template](https://github.com/gelatodigital/gelato-polywrap-template).

You'll also need the following installed before building your wrapper:

* `yarn`
* `docker`
* `docker-compose`

Once you have cloned the repository, do `yarn install` to install dependencies.

The main files you will have to change are `src/query/index.ts` and `src/query/schema.graphql`

## Defining a schema

### `./src/query/schema.graphql`

Here, the only thing you should modify is `UserConfig` .&#x20;

Similar to smart contract resolvers, you can also pass arguments to Polywrap resolvers. If you would like to do so, you have to define the argument types here.&#x20;

![](<../../.gitbook/assets/Screenshot 2021-11-07 at 8.47.44 PM.png>)

If you do not need any arguments, you can just leave it empty like so.

![](<../../.gitbook/assets/Screenshot 2021-11-07 at 9.49.25 PM.png>)

## Writing your resolver

### `./src/query/index.ts`

You will define the logic of your resolver in this file. Like writing asse

![](<../../.gitbook/assets/Screenshot 2021-11-14 at 12.35.16 PM.png>)

Here we deserialize the argument that we are passing if any. In this example `counterAddress` .

Another parameter that is passed in the argument is `ethConnection` which is needed for the Ethereum Plugin that is covered below.&#x20;

## Gelato Supported Plugins

### Ethereum Plugin

The Ethereum plugin is the EtheresJs plugin which can be used inside your Polywrap resolver.&#x20;

Check out the schema [here](https://github.com/polywrap/monorepo/blob/2947f956485decb43363f42c99c2a6176a25bde8/packages/js/plugins/ethereum/schema.graphql#L94-L173) to get an idea of the functions that are available.

We can use `callContractView` to call view functions like so.&#x20;

```typescript
  const lastExecuted = Ethereum_Query.callContractView({
    address: COUNTER,
    method: "function lastExecuted() view returns (uint256)",
    args: null,
    connection: ethConnection,
  });
```

### Subgraph Plugin

Allows you to query subgraphs in your Polywrap resolver.

Check out the schema [here](https://github.com/polywrap/monorepo/blob/2947f956485decb43363f42c99c2a6176a25bde8/packages/js/plugins/graph-node/schema.graphql#L3-L9).

Here is an example:

```typescript
 const res = GraphNode_Query.querySubgraph({
    subgraphAuthor: "gelatodigital",
    subgraphName: "poke-me-polygon",
    query: `{
      version(id:"3"){
        tasks{id}
      }
    }
    `,
  });
```

### HTTP Plugin

Allows you to do HTTP requests in your Polywrap resolver.

Check out the schema [here](https://github.com/polywrap/monorepo/blob/2947f956485decb43363f42c99c2a6176a25bde8/packages/js/plugins/http/schema.graphql#L30-L33).

Here is an example:&#x20;

```typescript
  const res = HTTP_Query.get({
    url: url,
    request: null,
  });
```

### Logger Plugin

Allows you to log messages in your resolver which can be useful for debugging.

Here is an example:&#x20;

```typescript
 Logger_Query.log({
    level: Logger_Logger_LogLevel.INFO,
    message: `config.counterAddress : ${config.counterAddress}`,
  });
```

### DateTime Plugin

Allows you to get the current epoch UNIX timestamp in milliseconds.

Here is an example:

```typescript
const time = DateTime_Query.currentTime({})
```

