# Node Operators' Guide

This guide provides practical steps for the node operators to start an es-node instance for connecting to the existing EthStorage testnet.

Storage providers can join the L2 network permissionlessly by running an es-node, so that they become EthStorage node operators.

Node operators need to download the replica of the L2 data into their local node, submit proof of storage to the L1 EthStorage contract, and therefore get the corresponding reward once the proof is accepted by the contract. 

## Minimum Hardware Requirements 

The minimum hardware requirements for an es-node are as follows:

 - CPU: A minimum of 4 cores and 8 threads
 - 4GB of RAM
 - Disk: 
    - We recommend using an NVMe disk to support the full speed of sampling
    - At least 550 GB of available storage space for the runtime and sync of one data shard
 - Internet service: At least 8MB/sec download speed

## System Environment

 - Ubuntu 20.04+ (tested and verified)
 - (Optional) Docker 24.0.5+ (would simplify the process)
 - (Optional) Go 1.20+ and Node.js 16+ (can be installed following the [steps](#install-dependencies))

You can choose [the method of running es-node](#options-to-run-es-node) based on your current environment.

_Note: The steps assume the use of the root user for all command line operations. If using a non-root user, you may need to prepend some commands with "sudo"._

## Preparing miner and signer account

It is recommended to prepare two Ethereum accounts specifically for this test. One of these accounts should contain a balance of test ETH to be used as a transaction signer.

The test ETH can be requested from [https://sepoliafaucet.com/](https://sepoliafaucet.com/). 

Remember to use the signer's private key (with ETH balance) to replace `<private_key>` in the following steps. And use the other address to replace `<miner>`.


## About `run.sh`

To start an es-node, you have the option to run the pre-built executables, or manually built binary from source code, or with Docker. 

The `run.sh` script is used as an entry point in all the above options. The main function of the script is to initialize the data file, prepare for mining, and launch es-node with preset parameters. 

Mining is enabled by default by the `--miner.enabled` flag in `run.sh`, which means you become a storage provider when you start an es-node with default settings.


_Note: Some of the flags/parameters used in `run.sh` are supposed to change over time. Refer to [configuration](#configuration) for a full list._