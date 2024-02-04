---
description: What is the right way to retrieve the data from the EthStorage network?
---

# Get Data

## Limitations on get()

As the values of the KV entries are stored across different nodes, a consensus node may be unable to fulfill a get operation if it's invoked within a block transaction.

Consequently, we restrict the execution of get operations within the consensus process, such as block proposal or validation. However, a transaction can encompass the KV entry value and validate it on-chain using the verify() method.

## Get() by JSON-RPC

Despite the above limitations, a contract can still invoke the get operation within a JSON-RPC environment, specifically, using the eth call JSON-RPC method.&#x20;

The input argument of a JSON-RPC call may include a list of values that the contract can read, which are `kvIndex`, `blobHash`, `decodeType`, `offset`, and `size`.

For example:&#x20;

```
curl -X POST -H 'content-type: application/json' --data '{"jsonrpc":"2.0","method":"es_getBlob","params":[0,"0x0128659750f9e73215ee22ac2e5446b03ad047a41b3f4aa20000000000000000", 0, 0, 131072],"id":0}' http://localhost:9545
```

During contract execution, if the values are not found in the argument, the call will attempt to locate the value data locally. If the local data file also does not contain the data, the call will fail and return an error specifying the expected value (along with its key and index) to provide. This allows the caller to retrieve the values from another node and repeat the process until the call is successful.
