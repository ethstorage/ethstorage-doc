# Storage Provider Guide

A storage provider in the EthStorage network is an active participant responsible for securely storing data. They store blob data on behalf of the network and receive storage fees as rewards for their service.

## Become a Storage Provider

Within the EthStorage network, storage providers operate storage nodes (a.k.a. es-node) to download and store data of their interest, subsequently receiving rewards distributed by the storage contract.

The storage providers must provide proof of replication over time to collect rewards. This process is sometimes called "mining", and the storage provider is called a "miner".

Follow the [tutorial](/storage-provider-guide/tutorials.md) to run a node and start mining.

## Join the EthStorage Mainnet before getting Whitelisted

During the initial launch of EthStorage Mainnet, participation as a storage provider is limited to whitelisted operators. Once the network reaches stability, it will transition to a fully permissionless model.

If you haven't been whitelisted yet, you can still join the network as a storage provider with mining temporarily disabled. Just follow the [tutorial](/storage-provider-guide/tutorials.md) to run an es-node as a normal operator—simply append `--miner.enabled=false` to any command that uses `./run-mainnet.sh` (or `./run.sh` for Sepolia testnet).

Once your miner account is whitelisted or the restriction is lifted, simply remove the `--miner.enabled=false` flag to enable mining.