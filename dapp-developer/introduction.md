# Introduction

EthStorage offers a CRUD (create, read, update, delete) interface, enabling operations on a key-value store (KV-store), where each value is referred to as a blob.

## Create or Update
 
```
putBlob(bytes32 key, uint256 blobIdx, uint256 length)
```

 This function writes the key-value pair to the underlying decentralized KV store. If the KV pair already exists, it will be overridden; otherwise, it will be appended to the KV array.
  * `key`: The key of the key-value pair to be stored.
  * `blobIdx`: The index of the blob within the blob-carrying transaction. It is utilized to obtain the commitment of the blob with the DATAHASH opcode.
  * `length`: The size of the value, which must be smaller than or equal to the size of a blob (131072).

## Read Data 

```
get(bytes32 key, DecodeType decodeType, uint256 off, uint256 len)
```

This function retrieves the requested data by key based on the specified offset and length. Note that the get method can only be called in the JSON-RPC context of the es-node of L2.
  * `key`: The key of the key-value pair whose value is to be accessed.
  * `decodeType`: An enumeration value indicating how the blob value should be decoded. It must be 0, representing `RawData` or 1, representing `PaddingPer31Bytes`.
  * `off`: The offset position within the blob denotes the starting point from which the data should be read.
  * `len`: The size of the value to be read.

### Limitations on get()

As the values of the KV entries are stored across different nodes, a consensus node may be unable to fulfill a get operation if it's invoked within a block transaction.

Consequently, we restrict the execution of get operations within the consensus process, such as block proposal or validation. However, a transaction can encompass the KV entry value and validate it on-chain using the verify() method.

### Get by JSON-RPC

Despite the above limitations, a contract can still invoke the get operation within a JSON-RPC environment, specifically, using the eth call JSON-RPC method.

The input argument of a JSON-RPC call may include a list of values that the contract can read, which are `kvIndex`, `blobHash`, `decodeType`, `offset`, and `size`.

For example:&#x20;

```
curl -X POST -H 'content-type: application/json' --data '{"jsonrpc":"2.0","method":"es_getBlob","params":[0,"0x0128659750f9e73215ee22ac2e5446b03ad047a41b3f4aa20000000000000000", 0, 0, 131072],"id":0}' http://localhost:9545
```

During contract execution, if the values are not found in the argument, the call will attempt to locate the value data locally. If the local data file also does not contain the data, the call will fail and return an error specifying the expected value (along with its key and index) to provide. This allows the caller to retrieve the values from another node and repeat the process until the call is successful.


## Delete

```
remove(bytes32 key)
```
 (_Not implemented_) This function deletes an existing key-value pair and refunds the associated costs.
  * `key`: The key of the key-value pair to be removed.

