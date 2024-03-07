---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Key Terms

## Ethereum DA

Layer 2 scaling solutions, such as rollups, reduce transaction costs and increase Ethereum's throughput by processing transactions off-chain and then posting them to the Ethereum mainnet in compressed batches. To Ethereum, the data availability problem is the need to prove to the whole network that these summarized form of transaction data really represents a set of valid transactions.

## Danksharding & EIP-4844

Danksharding is the full realization of the rollup scaling that will bring massive amounts of space on Ethereum for rollups to dump their compressed transaction data. EIP-4844 (a.k.a. Proto-Danksharding), as an intermediate step, introduces data blobs that can be sent and attached to blocks. The data in these blobs is not accessible to the EVM and is optionally deleted after a fixed period, which offers a more cost-effective solution compared to storing the data in calldata.


## Shard

As the entire storage dataset can be potentially huge, it is evenly divided into multiple fixed-size partitions that are consecutively numbered, known as shards. Each shard contains non-overlapping data with independent mining parameters within the storage contract.

A shard is sized appropriately so that it can be hosted by a home computer, e.g., 512GB or 1TB.

