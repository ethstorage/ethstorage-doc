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

For example, the following URL retrieves the 3rd blob sidecar in the block of slot 4756895.

```sh
http://65.109.50.145:9645/eth/v1/beacon/blob_sidecars/4756895?indices=3
```

The difference in returned data between es-node archiver API and beacon API is that for each blob es-node archiver only returns blob content, index, KZG commitment, and KZG proof. Other elements are omitted according to how the OP Stack uses the service.

```json
{
  "data": [
    {
      "index": "3",
      "kzg_commitment": "0xb93ab7583ad8a57b2edd262889391f37a83ab41107dc02c1a68220841379ae828343e84ac1c70fb7c2640ee3522c4c36",
      "kzg_proof": "0x86ffb073648261475af77cc902c5189bf3d33d0f63e025f23c69ac1e4cc0a7646e1a59ff8e5600f0fcc35f78fe1a4df2",
      "blob": "0x0068656c6c6..."
    }
  ]
}
```