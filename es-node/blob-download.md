---
description: Download new blobs from L1 via EIP-4844 specs, and synchronize expired ones from other es-node peers.
---

# Blob Download

## Download blobs

As designed in EIP-4844, blobs are stored in beacon nodes rather than in the execution layer (e.g., in Lighthouse, not in Geth). They remain accessible for a significant duration, enabling all participants within an L2 network to retrieve them. Moreover, their transient nature ensures that disk usage remains manageable, thereby enabling more cost-effective pricing for these blobs.

An essential task of the es-node is to monitor the input data of the storage contract and retrieve all newly arrived data blobs managed by the storage contract on the Ethereum beacon chain.

When a KV store is created or updated in the contract, a `PutBlob` event is triggered, providing details about the blob, including the global blob index, data hash, and blob size. Upon receiving the event, the es-node queries the blob data from the beacon chain node at the slot when the event is emitted.

The overall process of downloading entails two phases: initially caching the blobs when they become available from a beacon node, and subsequently writing the blobs into the shard file once the corresponding blocks are finalized. When writing the blobs into the shard file, they will be encoded with the data hash, blob index, and the miner's address.
 
 ## Data sync
Those blobs longer than 4096 epochs (~18 days) may no longer retained by L1 nodes. So a newly started es-node will get the outdated blobs from other p2p nodes.
