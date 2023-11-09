# Shard

The entire storage dataset is evenly divided into multiple fixed-size partitions known as shards. 

A shard is sized appropriately so that it can be hosted by a home computer, e.g., 1TB or 4TB. Each shard contains non-overlapping data with independent mining parameters within the storage contract.

Each node can host one or more shards and can claim rewards for each shard by regularly submitting proof of storage. As the number of KV entries grows, new shards will be dynamically created to accommodate the additional values of KV entries.

Upon node launch, the storage provider can freely select which shards to host, typically based on the shards offering the most anticipated rewards. This often means selecting shards with a lower number of replicas according to the oracle.