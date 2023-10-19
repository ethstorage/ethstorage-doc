# Proof of Publication

EthStorage leverages Ethereum data availability technologies to achieve the goal of proof of publication.

## Ethereum Data Availability

Layer 2 scaling solutions, such as rollups, reduce transaction costs and increase Ethereum's throughput by processing transactions off-chain and then posting them to the Ethereum mainnet in compressed batches. To Ethereum, the data availability problem is the need to prove to the whole network that these summarized form of transaction data really represents a set of valid transactions. 

Data Availability Sampling (DAS) is a way for the network to check that data is available without putting too much strain on any individual node. This relies upon data erasure coding solutions such as Reed-Solomon codes, which expands a given dataset with redundant information that allows the original data to be recovered when necessary. 

Danksharding is the full realization of the rollup scaling that will bring massive amounts of space on Ethereum for rollups to dump their compressed transaction data. Proto-Danksharding (a.k.a.EIP-4844), as an intermediate step along the way, introduces  data blobs that can be sent and attached to blocks. The data in these blobs is not accessible to the EVM and is automatically deleted after a fixed time period, which offers a more cost-effective compare to the calldata.

KZG is a proof scheme that reduces a blob of data down to a small cryptographic commitment, and fits a polynomial equation to the data. The blob of data submitted by a rollup has to be verified using KZG commitments that evaluates the polynomial at some secret data points. 

Data Upload with DA

