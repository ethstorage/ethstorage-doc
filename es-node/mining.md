# Mining

## Proof of Random Access (PoRA)

The crucial factor in achieving decentralized storage for large dynamic datasets is the construction of an on-chain oracle to estimate the number of replicas for each shard. Nodes that demonstrate the replication of the data shard over time are then rewarded. The reward for each shard is dispersed to nodes that can prove the replication via the process of Proof of Random Access (PoRA).

The Proof of Random Access is a mining process that heavily depends on read input/output operations performed on shard data. Like proof of work mining, the storage contract keeps a dynamic difficulty parameter for each shard and adjusts it after accepting the submission result of the PoRA. 
Therefore, we can estimate the number of replicas of the shard by comparing the hash rate (read IO rate) to the read IO rate of the most mining-economical storage devices (e.g., 1TB NVME SSD).

## Proof of Replication

To encourage nodes to host the data of each shard, we utilize a proof of replication algorithm based on proof of random access. The objective of the proof of random access is to provide an on-chain oracle with information about the number of read input/output operations (in MIN IO SIZE terms, e.g., 4096 for most SSDs) performed over the shard data over time. 
For instance, if the oracle reports 10,000,000 4KB read IOs per second and a typical NVME SSD can provide 500,000 4KB read IOs per second, we can estimate that approximately 20 disks are hosting the shard data in the network.





