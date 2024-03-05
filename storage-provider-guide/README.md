# Storage Provider Guide

A storage provider in the EthStorage network is an active participant responsible for securely storing data. They store blob data on behalf of the network and receive storage fees as rewards for their service.

## Become a Storage Provider

Within the EthStorage network, storage providers operate storage nodes (a.k.a. es-node) to download and store data of their interest, subsequently receiving rewards distributed by the storage contract.

The storage providers must provide proof of replication over time to collect rewards. This process is sometimes called "mining", and the storage provider is called a "miner".

Becoming a storage provider in the EthStorage network is fully permissionless, provided the hardware requirements are satisfied.

## Minimum Hardware Requirements

The minimum hardware requirements for an es-node are as follows:

* CPU: A minimum of 4 cores and 8 threads
* 4GB of RAM
* Disk:
  * We recommend using an NVMe disk to support the full speed of sampling
  * At least 550 GB of available storage space for the runtime and sync of one data shard
* Internet service: At least 8MB/sec download speed

##
