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

# Tutorial

This guide provides practical steps for the storage providers to start an es-node instance for connecting to the existing EthStorage testnet.

## Before Starting

### Minimum Hardware Requirements

* CPU: A minimum of 4 cores and 8 threads
* 8GB of RAM
* Disk:
  * We recommend using an NVMe disk to support the full speed of sampling
  * At least 550 GB of available storage space for the runtime and sync of one data shard
* Internet service: At least 8MB/sec download speed

### System Environment

* MacOS Version 14+, Ubuntu 20.04+ or Windows with WSL
* (Optional) Docker 24.0.5+ (would simplify the process)
* (Optional) Go 1.20+ and Node.js 16+ (can be installed following the [steps](tutorials.md#install-dependencies))

_Note: The steps assume the use of the root user for all command line operations. If using a non-root user, you may need to prepend some commands with "sudo"._

### Preparing miner and signer account

It is recommended to prepare two specific Ethereum accounts for this test. For safety reasons, we strongly suggest creating two new wallets to avoid the loss of any personal assets.

One account will act as a transaction signer and should contain a balance of test ETH, which can be requested from [https://sepoliafaucet.com/](https://sepoliafaucet.com/).

The other account will serve as the miner address, set to receive rewards once the storage provider successfully submits a storage proof to the EthStorage contract.

Remember to use the signer's private key (with ETH balance) to replace `<private_key>` in the following steps. And use the other address to replace `<miner>`.

### About `run.sh`

The `run.sh` script is used as an entry point. The main function of the script is to initialize the data file, prepare for mining, and launch es-node with preset parameters.

Mining is enabled by default by the `--miner.enabled` flag in `run.sh`, which means you become a storage provider when you start an es-node with default settings.

_Note: Some of the flags/parameters used in `run.sh` are supposed to change over time. Refer to_ [_configuration_](/storage-provider-guide/configuration.md) _for a full list._

## Options for running es-node

You can run es-node from a pre-built executable, a pre-built Docker image, or from the source code.

* If you choose [the pre-built es-node executable](tutorials.md#from-pre-built-executables), you will need to manually install some dependencies such as [Node.js](/storage-provider-guide/tutorials.md#install-nodejs) and [snarkjs](/storage-provider-guide/tutorials.md#install-snarkjs).
* If you have Docker version 24.0.5 or above installed, the quickest way to get started is by [using a pre-built Docker image](tutorials.md#from-a-docker-image).
* If you prefer to build [from the source code](tutorials.md#from-source-code), you will also need to install Go besides Node.js and snarkjs.


### From pre-built executables

Before running es-node from the pre-built executables, ensure that you have installed [Node.js](tutorials.md#install-nodejs), [snarkjs](tutorials.md#install-snarkjs). You also need to install [WSL](https://learn.microsoft.com/en-us/windows/wsl/install) if you are on Windows.

Download the pre-built package suitable for your platform:

Linux x86-64 or WSL (Windows Subsystem for Linux):

```sh
curl -L https://github.com/ethstorage/es-node/releases/download/v0.1.8/es-node.v0.1.8.linux-amd64.tar.gz | tar -xz
```

MacOS x86-64:

```sh
curl -L https://github.com/ethstorage/es-node/releases/download/v0.1.8/es-node.v0.1.8.darwin-amd64.tar.gz | tar -xz
```

MacOS ARM64:

```sh
curl -L https://github.com/ethstorage/es-node/releases/download/v0.1.8/es-node.v0.1.8.darwin-arm64.tar.gz | tar -xz
```

Run es-node

```
cd es-node.v0.1.8
env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> ./run.sh
```

### From a Docker image

Run an es-node container in one step:

```sh
docker run --name es  -d  \
          -v ./es-data:/es-node/es-data \
          -e ES_NODE_STORAGE_MINER=<miner> \
          -e ES_NODE_SIGNER_PRIVATE_KEY=<private_key> \
          -p 9545:9545 \
          -p 9222:9222 \
          -p 30305:30305/udp \
          --entrypoint /es-node/run.sh \
          ghcr.io/ethstorage/es-node:v0.1.8
```

You can check docker logs using the following command:

```sh
docker logs -f es 
```

### From source code

You will need to [install Go](tutorials.md#install-go) to build es-node from source code, and install [Node.js](tutorials.md#install-nodejs) and [snarkjs](tutorials.md#install-snarkjs) to run es-node.

Download source code and switch to the latest release branch:

```sh
git clone https://github.com/ethstorage/es-node.git
cd es-node
git checkout v0.1.8
```

Build es-node:

```sh
make
```

Start es-node

```sh
chmod +x run.sh && env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> ./run.sh
```

With source code, you also have the option to build a Docker image by yourself and run an es-node container:

```sh
env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> docker-compose up 
```

If you want to run Docker container in the background and keep all the logs:

```sh
env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> ./run-docker.sh
```

## Install dependencies

_Please note that not all steps in this section are required; they depend on your_ [_choice_](tutorials.md#options-for-running-es-node)_._

### Install Go

Download a stable Go release, e.g., go1.21.4.

```sh
curl -OL https://golang.org/dl/go1.21.4.linux-amd64.tar.gz
```

Extract and install

```sh
tar -C /usr/local -xf go1.21.4.linux-amd64.tar.gz
```

Update `$PATH`

```
echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.profile
source ~/.profile
```

### Install Node.js

Install Node Version Manager

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

Close and reopen your terminal to start using nvm or run the following to use it now:

```sh
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Choose a Node.js version above 16 (e.g. v20.\*) and install

```sh
nvm install 20
```

Activate the Node.js version

```sh
nvm use 20
```

### Install snarkjs

```sh
npm install -g snarkjs
```

## Check the status after launching the es-node

It's important to monitor the node closely until it successfully submits its first storage proof. Typically, the process encompasses three main stages.

### Data sync phase

The es-node will synchronize data from other peers in the network. You can check from the console the number of peers the node is connected to and, more importantly, the estimated syncing time.

During this phase, the CPUs are expected to be fully occupied for data processing. If not, please refer to [the FAQ](/storage-provider-guide/storage-provider-faq.md#how-to-tune-the-performance-of-syncing) for performance tuning on this area.

A typical log entry in this phase appears as follows:

```
INFO [01-18|09:13:32.564] Storage sync in progress progress=85.50% peerCount=2 syncTasksRemain=1@0 blobsSynced=1@128.00KiB blobsToSync=0 fillTasksRemain=30 emptyFilled=3,586,176@437.77GiB emptyToFill=608,127   timeUsed=1h48m7.556s  eta=18m20.127s

```

### Sampling phase

Once data synchronization is complete, the es-node will enter the sampling phase, also known as mining.

A typical log entry of sampling during a slot looks like this:

```
INFO [01-19|05:02:23.210] Mining info retrieved                    shard=0 LastMineTime=1,705,634,628 Difficulty=4,718,592,000 proofsSubmitted=6
INFO [01-19|05:02:23.210] Mining tasks assigned                    shard=0 threads=64 block=10,397,712 nonces=1,048,576
INFO [01-19|05:02:26.050] The nonces are exhausted in this slot, waiting for the next block samplingTime=2.8s shard=0 block=10,397,712

```

When you see "The nonces are exhausted in this slot...", it indicates that your node has successfully completed all the sampling tasks within a slot. The "samplingTime" value informs you of the duration, in seconds, it took to complete the sampling process.

If the es-node doesn't have enough time to complete sampling within a slot, the log will display "Mining tasks timed out". For further actions, please refer to [the FAQ](/storage-provider-guide/storage-provider-faq.md#what-can-i-do-about-mining-tasks-timed-out).

### Proof submission phase

Once the es-node calculates a valid storage proof, it will submit the proof to the EthStorage contract and receive the rewards.

A typical log entry of submitting proof looks like this:

```
INFO [01-19|05:02:23.210] Calculated a valid hash                  shard=0 thread=3 block=5,437,371 nonce=58101
INFO [01-19|05:05:23.210] Mining transaction confirmed"            txHash=0x7afa13e5211c403a7024bdf0a6880203d54698355679be9aab1aa0ecef78eecd
INFO [01-19|05:05:23.210] Mining transaction success! √            miner=0xBB9D13efa21c0a053eCFE622e2AbfAF0D7573f50
```

Once you see this log, congratulations on completing the entire process as a storage provider. You can also check how many storage proofs you've submitted, your ETH profit, and your ranking on the [dashboard](https://grafana.ethstorage.io).