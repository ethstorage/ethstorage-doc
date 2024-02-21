---
description: >-
  The solution to scaling Ethereum storage proposed by EthStorage is to build an
  external Layer 2 data retention network that is fully decentralized and
  permissionless.
---

# External Data Retention L2 Network

The EthStorage network comprises two primary components: a storage contract deployed on the Ethereum mainnet and L2 storage nodes, such as es-node, responsible for actual data storage.

## Storage contract

A storage contract has been deployed on the Ethereum mainnet to accept Proofs of Storage to efficiently verify that the data is stored in the storage nodes.

It is important to note that the storage contract does not store the full values referred to by the keys. Instead, it stores data hashes that are derived from KZG commitments to blobs. Additionally, this contract provides Key-Value CRUD semantics, including operations such as put, get, and delete, in addition to verifying proofs.

Please refer to [storage contract](https://github.com/ethstorage/ethstorage-doc/blob/master/storage-contract/README.md) for detail.

## Storage node

L2 storage nodes like es-node provide various functionalities, including:

* [Downloading blobs](https://github.com/ethstorage/ethstorage-doc/blob/master/overview/technologies/broken-reference/README.md) from an L1 node: L2 storage nodes can retrieve blobs from Ethereum beacon chain using the EIP-4844 specifications. This enables the transfer of data between the two layers.
* [Synchronizing](https://github.com/ethstorage/ethstorage-doc/blob/master/overview/technologies/broken-reference/README.md) expired blobs from other peers: A peer-to-peer network is established by L2 storage nodes to discover and synchronize data between the nodes effectively. By synchronizing blobs from other peers, L2 storage nodes ensure that newly joined nodes have access to all historical blobs in the network.
* [Encoding](https://github.com/ethstorage/ethstorage-doc/blob/master/overview/technologies/broken-reference/README.md) and storing blobs: L2 storage nodes encode the received blobs and store them in their local storage. Encoding makes sure the storage provider stores a unique replica of the data physically.
* [Generating proof of storage by sampling the encoded blobs](https://github.com/ethstorage/ethstorage-doc/blob/master/overview/technologies/broken-reference/README.md): L2 storage nodes generate a proof of storage by sampling the encoded blobs. This proof ensures that the nodes have indeed stored the blobs and can provide them when required.
* [Submitting proof of storage](https://github.com/ethstorage/ethstorage-doc/blob/master/overview/technologies/broken-reference/README.md): L2 storage nodes submit the generated proof of storage to the storage contract to validate their participation in the network. And this is how the storage providers collect their reward once the proof is accepted by the contract.

These functionalities collectively enable decentralized data retrieval, storage, and verification in the L2 data retention network. Storage providers can join the L2 network permissionlessly by running a storage node.
