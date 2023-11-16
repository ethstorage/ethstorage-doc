---
description: >-
  The storage smart contract is designed to manage the data in the form of blobs
  introduced by EIP-4844 on Ethereum.
---

# Store Data by Using Blobs

## The management of the blobs&#x20;

The system is designed to accommodate a substantial number of key-value pairs in the store. Consequently, the storage contract retains the comprehensive metadata of the KV store, supporting essential functionalities like ownership management, key to physical address translation, and data hashes for integrity checks.&#x20;

On the other hand, the blobs themself are stored as a sizable byte array in storage nodes. Given the array's potential magnitude—totaling tens or hundreds of terabytes or even petabytes—the array content will be divided into multiple shards distributed across different nodes in the network.

The contract indexes all blobs from 0 to n-1, where n is the total number of blobs, to facilitate random sampling for verification purposes. It integrates with the EIP-4844 point-evaluation precompile to verify proofs of continuous random sampling of stored data submitted by storage nodes.

## The semantics of the KV store

The Ethereum DA empowers EthStorage to offer a CRUD (create, read, update, delete) interface, enabling operations on a key-value store (KV-store), where each value is referred to as a blob.&#x20;

* `putBlob(bytes32 key, uint256 blobIdx, uint256 length)` This function writes the key-value pair to the underlying decentralized KV store. If the KV pair already exists, it will be overridden; otherwise, it will be appended to the KV array.
  * `key`: The key of the key-value pair to be stored.
  * `blobIdx`: The index of the blob within the blob-carrying transaction. It is utilized to obtain the commitment of the blob with the DATAHASH opcode. For further details, please refer to [this link](https://github.com/ethstorage/eip4844-blob-hash-getter).
  * `length`: The size of the value, which must be smaller than or equal to the size of a blob (131072).
* `get(bytes32 key, DecodeType decodeType, uint256 off, uint256 len)` This function retrieves the requested data by key based on the specified offset and length. Note that the get method can only be called in the JSON-RPC context of the es-node of L2. Please refer to this page for more information. Please refer to [this page](get-data.md) for more information.
  * `key`: The key of the key-value pair whose value is to be accessed.
  * `decodeType`: An enumeration value indicating how the blob value should be decoded. It must be 0, representing `RawData` or 1, representing `PaddingPer31Bytes`.
  * `off`: The offset position within the blob denotes the starting point from which the data should be read.
  * `len`: The size of the value to be read.
* `remove(bytes32 key)` This function deletes an existing key-value pair and refunds the associated costs.
  * `key`: The key of the key-value pair to be removed.

## Benefits of using Ethereum blobs:

In addition to achieving similar Key-Value (KV) CRUD semantics as SSTORE, storing data using Ethereum blobs offers the following benefits:

* Security: Leveraging Ethereum mainnet security provides resilience against block re-organization, ensures secure storage cost settlement, and enhances censorship resistance.
* Programmability: Smart contracts can programmatically manage storage, facilitating the easy implementation of new features such as multi-user access control and data composability.
* Zero onboarding cost: As EthStorage is built on top of Ethereum, storage costs are paid in ETH, allowing users to perform storage operations using ETH wallets like Metamask without the need to learn new tokens, wallets, or addresses.
* Atomicity with application logic and storage logic: A classic web3 application often involves two steps: 1) uploading the data to an external storage network and 2) storing the content hash on the chain. Through the integration of DA and EVM, EthStorage can execute both application and storage logic within a single transaction, simplifying operations and improving user experience, a trait commonly found in leading Web2 applications such as X and Meta.
