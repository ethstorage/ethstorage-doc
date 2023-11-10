# Shard

## Dynamic data sharding

EthStorage utilizes dynamic data sharding to achieve decentralized storage for large, dynamic datasets, particularly for large decentralized key-value (KV) stores. The values of all KV entries are stored in a large byte array. As the entire storage dataset can be potentially huge, it is evenly divided into multiple fixed-size partitions that are consecutively numbered, known as shards.

## Shard and es-node

A shard is sized appropriately so that it can be hosted by a home computer, e.g., 1TB or 4TB. Each shard contains non-overlapping data with independent mining parameters within the storage contract.

Each node can host one or more shards and can claim rewards for each shard by regularly submitting proof of storage. As the number of KV entries grows, new shards will be dynamically created to accommodate the additional values of KV entries.

Upon node launch, the storage provider can freely select which shards to host, typically based on the shards offering the most anticipated rewards. This often means selecting shards with a lower number of replicas according to the oracle.


## New shard

When the number of KV entries exceeds the limit of the latest shard (e.g., with ID=i), a new shard with the ID i+1 is dynamically created. However, a challenge with this new shard is that it may not have any nodes configured to serve it at the moment of creation due to the lack of incentive, meaning there is no mining reward just before its creation. As a result, a newly added KV entry in the new shard may be lost.

To address this issue, one potential solution is to pre-pay a certain amount of KV entries in the KV store contract. This pre-payment creates a grace period to incentivize nodes to join the shard to be created and mine the data, even if the shard hardly contains any KV entries. 

It is worth noting that some empty values will be added to the shard to ensure that Proof of Random Access can still function effectively, a practice sometimes referred to as "filling empty".