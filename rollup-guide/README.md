# EthStorage and Rollups

As a long-term DA solution built on Ethereum, EthStorage enables rollups to upload blobs containing L2 transaction data to its storage network via a customized batch inbox contract. Subsequently, these blobs can be retrieved as needed through the blob archiver API, even after they have expired on L1.

## Integration with the OP Stack

Today, the OP Stack releases greatly simplify the process of deploying new L2 Rollups. The list of names categorized under "OP Chain" is rapidly expanding, including Base, Mantle, Manta, Worldcoin, etc., in addition to the OP Mainnet itself. The OP Stack is the standardized, shared, and open-source development stack that was built as a public good for the Ethereum and Optimism ecosystems. The term OP Stack can be referred to as software components that either help define a specific layer of the Optimism ecosystem or fill a role as a module within an existing layer. 

The Data Availability Layer, among the major layers, defines where the raw inputs to an OP Stack based chain are published. Currently, Ethereum DA is widely used as DA module of this layer because an OP Stack chain can be derived from calldata or blobs accessible on the Ethereum blockchain, which is the safest option compared to the alt DA solutions such as Celestia, EigenDA, and Avail. The Bedrock release of the OP Stack submits L2 blocks as transaction calldata, which is expensive and not scalable. The Ecotone network upgrade supports posting transaction batches to the L1 using EIP-4844 blobs, and introduced a dramatic (up to ~100x) reduction in overall L2 transaction fees on OP Mainnet.

The primary issue with the blobs, however, is that they are stored for only approximately 18 days (4096 epochs) on L1, but the data contained in the expired blobs is still essential for the derivation of the L2 chain, such as for a newly initiated full node. 

Thanks to its modularized design, an OP Stack chain can use an additional DA module to allow querying of all historical blobs, and the EthStorage archiver API is perfectly suited for this role.

## Introduction to EthStorage Archiver API

As a component of es-node, the archiver API serves as an additional fallback resource alongside the beacon API. 

It provides blob data similar to the [getBlobSidecars](https://ethereum.github.io/beacon-APIs/#/Beacon/getBlobSidecars) beacon API, offering the same interface (`/eth/v1/beacon/blob_sidecars/{block_id}`) to retrieves blob sidecars for a given block id. If the `indices` parameter is specified, only the blob sidecars with the specified indices will be returned. 

For example, the following URL retrieves the 3rd blob sidecar in the block of slot 8485244.

```sh
https://archive.testnet.ethstorage.io:9635/eth/v1/beacon/blob_sidecars/8485244?indices=4
```

The difference in returned data between es-node archiver API and beacon API is that for each blob es-node archiver only returns blob content, index, KZG commitment, and KZG proof. Other elements are omitted according to how the OP Stack uses the service.

```json
{
  "data": [
    {
      "index": "4",
      "kzg_commitment": "0x851c08e90cce0c09abe1e038f8368279477ac8dd88b5e240989fe6f1c8526a9f786dcf4ea1200dacba4e60bc2fbfc49e",
      "kzg_proof": "0x85202ce6625c1a31fe8e1a2301ddc0eaf731ab9890fe45fea850c6efe199c1253f905c11432f6f72500b654667d21ccc",
      "blob": "0x01000043c6ffd8ffe00010..."
    }
  ]
}
```