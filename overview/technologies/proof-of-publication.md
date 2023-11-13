# Proof of Publication

EthStorage leverages Ethereum data availability technologies to achieve the goal of proof of publication.

## Ethereum Data Availability

Layer 2 scaling solutions, such as rollups, reduce transaction costs and increase Ethereum's throughput by processing transactions off-chain and then posting them to the Ethereum mainnet in compressed batches. To Ethereum, the data availability problem is the need to prove to the whole network that these summarized form of transaction data really represents a set of valid transactions. 

Data Availability Sampling (DAS) is a way for the network to check that data is available without putting too much strain on any individual node. This relies upon data erasure coding solutions such as Reed-Solomon codes, which expand a given dataset with redundant information that allows the original data to be recovered when necessary. 

Danksharding is the full realization of the rollup scaling that will bring massive amounts of space on Ethereum for rollups to dump their compressed transaction data. EIP-4844 (a.k.a. Proto-Danksharding), as an intermediate step, introduces data blobs that can be sent and attached to blocks. The data in these blobs is not accessible to the EVM and is automatically deleted after a fixed period, which offers a more cost-effective compare to the calldata.

KZG is a proof scheme that reduces a blob of data to a small cryptographic commitment, and fits a polynomial equation to the data. The blob of data submitted by a rollup has to be verified using KZG commitments that evaluate the polynomial at some secret data points. 

## Bring Data to EthStorage

How can Ethereum DA solutions, such as EIP-4844 and Danksharding, be implemented within EthStorage?

EIP-4844 introduces a novel transaction type known as blob-carrying transaction, which includes additional data in the form of blobs. These blobs are temporarily stored on the consensus layer and can be downloaded by storage providers of EthStorage to their local nodes (referred to as es-nodes), where they are saved in local files. This process comprises the initial acquisition of data stored by EthStorage.

With Danksharding, the increased upload speed of Ethereum to approximately 2.66 MBps, about 20 times faster than its current speed, enables Ethereum DA to potentially provide EthStorage with a substantial and reliable source of data, ensuring sufficient available storage capacity.

The main advantage of Danksharding is that nodes are not required to download and broadcast all the data. Instead, each node can verify that any portion of the data can be downloaded from the network through DAS. This approach aligns well with the dynamic sharding model of EthStorage, where nodes only need to download the data they are interested in, rather than downloading all of them.