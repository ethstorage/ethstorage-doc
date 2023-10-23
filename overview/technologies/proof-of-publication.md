# Proof of Publication

EthStorage leverages Ethereum data availability technologies to achieve the goal of proof of publication.

## Ethereum Data Availability

Layer 2 scaling solutions, such as rollups, reduce transaction costs and increase Ethereum's throughput by processing transactions off-chain and then posting them to the Ethereum mainnet in compressed batches. To Ethereum, the data availability problem is the need to prove to the whole network that these summarized form of transaction data really represents a set of valid transactions. 

Data Availability Sampling (DAS) is a way for the network to check that data is available without putting too much strain on any individual node. This relies upon data erasure coding solutions such as Reed-Solomon codes, which expand a given dataset with redundant information that allows the original data to be recovered when necessary. 

Danksharding is the full realization of the rollup scaling that will bring massive amounts of space on Ethereum for rollups to dump their compressed transaction data. EIP-4844 (a.k.a. Proto-Danksharding), as an intermediate step, introduces data blobs that can be sent and attached to blocks. The data in these blobs is not accessible to the EVM and is automatically deleted after a fixed period, which offers a more cost-effective compare to the calldata.

KZG is a proof scheme that reduces a blob of data to a small cryptographic commitment, and fits a polynomial equation to the data. The blob of data submitted by a rollup has to be verified using KZG commitments that evaluate the polynomial at some secret data points. 

Data Upload with DA

How can Ethereum DA solutions, i.e., Proto-Danksharding and Danksharding, be applied to EthStorage? 

Proto-Danksharding introduces a unique transaction type called blob-carrying transaction that carries an extra piece of data blobs. The blobs are stored on the consensus layer for a short term, and storage providers of EthStorage can download them to their local nodes (a.k.a., es-node), where they are stored in local files. This is how data stored by EthStorage initially comes from. 

With Danksharding, the upload speed of Ethereum is expected to increase to approximately 2.66 MBps, which is about 20 times faster than the current upload speed of Ethereum. This means Ethererum DA has the ability to bring 1.5TB per week which is available for EthStorage to save.

The main advantage of Danksharding is that nodes are not required to download and broadcast all the data. Instead, each node can verify that any portion of the data can be downloaded from the network through DAS. This approach aligns well with the dynamic sharding model of EthStorage, where nodes only need to download the data they are interested in, rather than downloading all of them.