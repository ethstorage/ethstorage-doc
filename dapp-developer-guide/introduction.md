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