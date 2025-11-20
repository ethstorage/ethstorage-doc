# EthStorage and Rollups

As a long-term DA solution built on Ethereum, EthStorage enables rollups to upload blobs containing L2 transaction data to its storage network via a customized batch inbox contract. Subsequently, these blobs can be retrieved as needed through the blob archiver API, even after they have expired on L1.

## Integration with the OP Stack

Today, the OP Stack releases greatly simplify the process of deploying new L2 Rollups. The list of names categorized under "OP Chain" is rapidly expanding, including Base, Mantle, Manta, Worldcoin, etc., in addition to the OP Mainnet itself. The OP Stack is the standardized, shared, and open-source development stack that was built as a public good for the Ethereum and Optimism ecosystems. The term OP Stack can be referred to as software components that either help define a specific layer of the Optimism ecosystem or fill a role as a module within an existing layer. 

The Data Availability Layer, among the major layers, defines where the raw inputs to an OP Stack based chain are published. Currently, Ethereum DA is widely used as DA module of this layer because an OP Stack chain can be derived from calldata or blobs accessible on the Ethereum blockchain, which is the safest option compared to the alt DA solutions such as Celestia, EigenDA, and Avail. The Bedrock release of the OP Stack submits L2 blocks as transaction calldata, which is expensive and not scalable. The Ecotone network upgrade supports posting transaction batches to the L1 using EIP-4844 blobs, and introduced a dramatic (up to ~100x) reduction in overall L2 transaction fees on OP Mainnet.

The primary issue with the blobs, however, is that they are stored for only approximately 18 days (4096 epochs) on L1, but the data contained in the expired blobs is still essential for the derivation of the L2 chain, such as for a newly initiated full node. 

Thanks to its modularized design, an OP Stack chain can use an additional DA module to allow querying of all historical blobs, and the EthStorage archiver API is perfectly suited for this role.

## Introduction to EthStorage Archiver API

As a component of es-node, the archiver API serves as an additional fallback resource alongside the beacon API. 

It provides blob data similar to the [getBlobs](https://ethereum.github.io/beacon-APIs/#/Beacon/getBlobs) beacon API, offering the same interface (`/eth/v1/beacon/blobs/{block_id}`) to retrieves blobs for a given block id. If the `versioned_hashes` parameter is specified, only the blobs with the specified versioned hashes will be returned. 

For example, the following URL retrieves one of the blobs in the block of slot 13059580 with versioned hash `0x0158420bd7b1f3c04a097694235c52659f31b479018ac56b2545727fab33712d`.

```sh
https://archive.testnet.ethstorage.io:9635/eth/v1/beacon/blobs/8921627?versioned_hash=0x0158420bd7b1f3c04a097694235c52659f31b479018ac56b2545727fab33712d
```

The difference in returned data between es-node archiver API and beacon API is that for each blob es-node archiver only returns blob content, while other elements are omitted according to how the OP Stack uses the service.

```json
{
 "data": [
    "0x1d0001fbfc0024af0f1de6920a1a3a8a10e56db4558800000001fbe4019b05c708156c6360faba4924b0ebef6a6cf96912d97f9e1f40c57473404fe7d8e237ae2343aca9a32855c4f9cbff9f12b41..."
 ]
}
```