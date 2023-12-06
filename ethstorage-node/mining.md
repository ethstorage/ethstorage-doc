---
description: What is mining, what is its purpose, and how does it work?
---

# Mining

## Storage provider (a.k.a. miner)

A storage provider in the EthStorage network is an active participant responsible for securely storing data. They store blob data on behalf of the network and receive storage fees as rewards for their service.

Within the EthStorage network, storage providers operate storage nodes (a.k.a. es-node) to download and store data of their interest, subsequently receiving rewards distributed by the storage contract. To collect rewards, the storage providers must provide proof of replication over time. This process sometimes is referred to as "mining", and the storage provider is called a "miner".

Becoming a storage provider in the EthStorage network is fully permissionless, provided the hardware requirements are satisfied.

## Proof of Random Access (PoRA)

To achieve decentralized storage for large dynamic datasets, a crucial factor is the construction of an on-chain oracle to estimate the number of replicas for each [shard](core-concept/shard.md). In other words, we utilize a proof of replication algorithm to encourage nodes to host the data of each shard. Nodes that demonstrate the replication of the data shard over time are then rewarded. The reward for each shard is dispersed to nodes that can prove the replication via the process of PoRA.

The objective of the PoRA is to provide information about the number of read IO operations (in MIN IO SIZE terms, e.g., 4KB for most SSDs) performed over the shard data over time. The PoRA is a mining process that heavily depends on read IO operations performed on shard data. Like proof of work mining, the storage contract keeps a dynamic difficulty parameter for each shard and adjusts it after accepting the submission result of the PoRA. Therefore, we can estimate the number of replicas of the shard by comparing the hash rate (read IO rate) to the read IO rate of the most mining-economical storage devices (e.g., 1TB NVME SSD).

For instance, if the oracle reports 10,000,000 4KB read IOs per second and a typical NVME SSD can provide 500,000 4KB read IOs per second, we can estimate that approximately 20 disks are hosting the shard data in the network.

When certain nodes of the shard leave the network, the hash rate of the shard decreases, incentivizing new nodes to join the shard to receive better rewards. This dynamic procedure ensures replication over time, preventing data loss, and achieving proof of storage.

## Data mask

While the system uses shards as the basis for mining, KV entry is the smallest storage unit, which is exactly a blob in the case of using Ethereum as layer 1. To prove the actual existence of the physical replicas of a blob in the network, the basic idea is to make sure the storage provider encodes the blob with a mask.

The mask is computed by taking the blob commitment, KV index, and miner address as inputs. These three pieces of information - blob commitment, KV index, and miner address - are used to uniquely identify the data being stored by providing the necessary context for the mask computation to fulfill its functions of verifying storage proofs, distributing rewards, and preventing Sybil attacks or mistaken verification of storage by unintended parties.

There are several considerations for the mask generation algorithm:

* While it is a computationally expensive function to calculate, it can be performed in under a second, allowing physical replicas to be updated in a timely manner.
* The function should be selected such that the cost of physical storage (including power costs and storage device lifetime costs) is significantly lower than the on-demand computation costs (i.e. raw data storage costs plus costs of computing the mask on demand and computation device lifetime costs).
* The function should allow for easy and efficient on-chain verification using zero-knowledge proofs (ZKP), to validate the mask function being correctly executed.

## The mining process

With the encoded local data by unique masks, we can now start the mining process.

For each Ethereum slot, storage providers specified by their miner address are allowed to randomly sample encoded blobs up to a fairly large number of times, with each sampling corresponding to a hash candidate.

If a hash candidate satisfies the [difficulty](core-concept/mining-diff.md) condition specified in the storage contract, it represents a valid mining solution, similar to proofs of work algorithms. At this stage, the node will generate two proofs: an inclusion proof that verifies the sample's presence in the randomly selected blob using KZG commitment, and a zero-knowledge proof that confirms the accurate application of the mask to encode the blob.

The storage provider can then submit the proofs to the storage contract on Layer 1 in a standard Ethereum transaction. If the proofs are verified successfully by the contract, the storage provider collects the storage rental fees for all blobs stored since their last submission. Refer to [this section](core-concept/storage-fee-and-reward.md#fee-distributor) for the details of the distribution of storage fees.

Finally, the difficulty parameter is adjusted for each submission, similar to the Ethereum difficulty adjustment algorithm.
