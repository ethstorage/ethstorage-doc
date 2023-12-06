---
description: >-
  Download new blobs from L1 via EIP-4844 specs, and synchronize expired ones
  from other es-node peers.
---

# Downloading Blobs

## Downloading new blobs

As designed in EIP-4844, blobs are stored in beacon nodes rather than in the execution layer (e.g., in Lighthouse, not in Geth). They remain accessible for a significant duration, enabling all participants within an L2 network to retrieve them. Moreover, their transient nature ensures that disk usage remains manageable, thereby enabling more cost-effective pricing for these blobs.

An essential task of the es-node is to monitor the input data of the storage contract and retrieve all newly arrived data blobs managed by the storage contract on the Ethereum beacon chain.

When a KV store is created or updated in the contract, a `PutBlob` event is triggered, providing details about the blob, including the global blob index, data hash, and blob size. Upon receiving the event, the es-node queries the blob data from the beacon chain node at the slot when the event is emitted.

The overall process of downloading entails two phases: initially caching the blobs when they become available from a beacon node, and subsequently writing the blobs into the shard file once the corresponding blocks are finalized. When writing the blobs into the shard file, they will be encoded with the data hash, blob index, and the miner's address.

## Syncing older blobs

Ethereum beacon nodes may only retain blobs for 4096 epochs which equates to around 18 days. When a new node joins the network, it will retrieve recent blobs from L1 peers. However, to access older historical data, the node is also able to leverage the L2 peer-to-peer network. This takes the form of a peer-to-peer network between es-nodes based on devp2p. The L2 nodes work to discover and synchronize blob data between each other. Through this synchronization process, L2 nodes are able to share their collective long-term blob stores. By discovering and downloading blobs from multiple L2 peers, the new node is able to rebuild a full local copy of the network's blob data - even blobs that exceed the L1 threshold.

In this way, the L2 layer facilitates permanent distributed storage and accessibility of all blobs across the lifetime of the network.
