---
description: >-
  The concept of mining difficulty is crucial for understanding the dynamic
  sampling process in the Proof of Random Access (PoRA).
---

# Mining diff

In contrast to the PoW mining process, PoRA heavily relies on read IOs performed over the shard data, rather than relying on hash power.

Within the storage contract, a dynamic difficulty parameter is maintained for each shard. During each slot, an es-node will read a couple of data samples from positions computed based on an on-chain random seed, and then calculate a mixed hash of the samples. This mixed hash is then compared to the expected difficulty, computed based on the difficulty parameter of the shard, similar to the process in PoW.

If a hash candidate satisfies the expected difficulty, a valid proof can be submitted and verified by the storage contract. The difficulty parameter for the shard will be adjusted after accepting the result of the PoRA based on the expected submission interval.

An important role of the difficulty is to estimate the IO rate of the shard. The dynamic adjustment of difficulty acts as an on-chain oracle, enabling the estimation of the number of replicas for each shard and facilitating the distribution of storage resources across shards in the network as required.
