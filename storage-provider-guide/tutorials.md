---
layout:
  width: default
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
  metadata:
    visible: true
---

# Storage Provider Tutorial

This guide provides practical steps for the storage providers to start an es-node instance for connecting to one of the existing EthStorage networks.

This tutorial targets EthStorage deployments on Ethereum L1 (Mainnet and Sepolia) and [QuarkChain](https://quarkchain.io) L2 testnet. Some steps differ among the three networks, and the variant of each shell command is provided.

If you simply need to upgrade your es-node instance to a newer version, please refer [here](storage-provider-faq.md#i-am-already-running-es-node.-how-can-i-update-it-to-a-newer-version).

## Before Starting

### Minimum Hardware Requirements

* CPU: A minimum of 4 cores and 8 threads
* 8 GB of RAM
* Disk:
  * We recommend using an NVMe disk to support the full speed of sampling
  * At least 550 GB of available storage space for the runtime and sync of one data shard
* Internet: At least 8 MB/sec download speed

### System Environment

* macOS Version 14+, Ubuntu 20.04+, or Windows with WSL (Windows Subsystem for Linux) version 2
* (Optional) Docker 24.0.5+ (would simplify the process)
* (Optional) Go 1.23+ and Node.js 16+ (can be installed following the [steps](tutorials.md#install-dependencies))

> ℹ️ _**Note:**_ The steps assume the use of the root user for all command line operations. If using a non-root user, you may need to prepend some commands with "sudo".

### Preparing miner and signer account

We recommend preparing two specific Ethereum accounts to run an es-node.

* The miner account will be set to receive rewards once the storage provider successfully submits a storage proof to the EthStorage contract. Each storage provider must use a unique miner account on the same network.
* The signer account signs transactions and must hold sufficient ETH for gas. If you run multiple es-nodes on the same network, use a separate signer account per node to avoid nonce conflicts.

> ℹ️ _**Note:**_ If Sepolia is used as L1, request test ETH from [sepoliafaucet.com](https://sepoliafaucet.com/); for QuarkChain L2 testnet, get custom gas tokens [here](https://docs.quarkchain.io/guides/app-developers/receive-tokens).

> :warning: _**Warning:**_ For safety reasons, we strongly suggest creating new wallets for the accounts to avoid the loss of any personal assets.

Remember to use the signer's private key (the one with ETH balance) to replace `<private_key>` in the following steps. And use the other address to replace `<miner>`.

### Preparing Ethereum API endpoints (L1 only)

During the operation of es-node that deployed on L1, frequent Ethereum API calls are made, including at the execution layer and the consensus layer (the beacon chain). Therefore, we need you to prepare endpoints for two types of calls. We recommend using `BlockPI` for the execution layer endpoint and `QuickNode` for the beacon endpoint.

For details on the application process for endpoints, please refer to [this section](tutorials.md#applying-for-ethereum-api-endpoints).

In the following tutorial, you will need to replace `<el_rpc>` for your execution layer endpoint, and `<cl_rpc>` for the beacon endpoint.

> ℹ️ _**Note:**_ You do not need to apply Ethereum API endpoints to run es-node for the QuarkChain L2 network.

## Options for running es-node

You can run es-node from a pre-built executable, a pre-built Docker image, or from the source code.

* If you choose [the pre-built es-node executable](tutorials.md#from-pre-built-executables), you may need to install [Node.js](tutorials.md#install-node.js) if using default zk prover implementation.
* If you have Docker version 24.0.5 or above installed, the quickest way to get started is by [using a pre-built Docker image](tutorials.md#from-a-docker-image).
* If you prefer to build [from the source code](tutorials.md#from-source-code), you will also need to install Go besides other dependencies.

### From pre-built executables

Before running es-node from the pre-built executables, ensure that you have installed [Node.js](tutorials.md#install-node.js) and [snarkjs](tutorials.md#install-snarkjs) (unless [`--miner.zk-prover-impl`](tutorials.md#about-the-option-of-zk-prover-implementation) flag is set to `2`).

> ℹ️ _**Note:**_ Ensure that you run the executables on [WSL 2](https://learn.microsoft.com/en-us/windows/wsl/install) if you are using Windows, and both Node.js and snarkjs are installed on WSL instead of Windows.

#### Download the pre-built package 

Choose the one that is suitable for your platform:

* Linux x86-64 or WSL:

```sh
OS_ARCH=linux-amd64
```

* MacOS x86-64:

```sh
OS_ARCH=darwin-amd64
```

* MacOS ARM64:

```sh
OS_ARCH=darwin-arm64
```
Download and unpack the specified pre-built package:
```sh
VERSION=v0.2.6
curl -L "https://github.com/ethstorage/es-node/releases/download/${VERSION}/es-node.${VERSION}.${OS_ARCH}.tar.gz" \
  | tar -xz && mv "es-node.${VERSION}" es-node
cd es-node

# Verify the binary version:
./build/bin/es-node --version
```
#### Initialize es-node

In the directory `es-node`, execute one of the following commands according to your network:

* Mainnet

```sh
env ES_NODE_STORAGE_MINER=<miner> ./init-mainnet.sh --l1.rpc <el_rpc>
```
* Sepolia

```sh
env ES_NODE_STORAGE_MINER=<miner> ./init.sh --l1.rpc <el_rpc>
```
* QuarkChain L2

```sh
env ES_NODE_STORAGE_MINER=<miner> ./init-l2.sh
```

#### Start es-node

> ℹ️ _**Note:**_ Append `--miner.enabled=false` to the following commands if the miner account is not yet whitelisted.

* Mainnet

```sh
env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> ./run-mainnet.sh --l1.rpc <el_rpc> --l1.beacon <cl_rpc>
```

* Sepolia

```sh
env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> ./run.sh --l1.rpc <el_rpc> --l1.beacon <cl_rpc>
```

* QuarkChain L2

```sh
env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> ./run-l2.sh
```

### From a Docker image

#### Initialize es-node

> ℹ️ _**Note:**_ If you are using Windows, execute the command in WSL

* Mainnet

```sh
docker run --rm \
          -v ./es-data:/es-node/es-data \
          -v ./zkey:/es-node/build/bin/snark_lib/zkey \
          -e ES_NODE_STORAGE_MINER=<miner> \
          --entrypoint /es-node/init-mainnet.sh \
          ghcr.io/ethstorage/es-node:v0.2.6 \
          --l1.rpc <el_rpc>
```

* Sepolia

```sh
docker run --rm \
          -v ./es-data:/es-node/es-data \
          -v ./zkey:/es-node/build/bin/snark_lib/zkey \
          -e ES_NODE_STORAGE_MINER=<miner> \
          --entrypoint /es-node/init.sh \
          ghcr.io/ethstorage/es-node:v0.2.6 \
          --l1.rpc <el_rpc>
```

* QuarkChain L2

```sh
docker run --rm \
          -v ./es-data:/es-node/es-data \
          -v ./zkey:/es-node/build/bin/snark_lib/zkey \
          -e ES_NODE_STORAGE_MINER=<miner> \
          --entrypoint /es-node/init-l2.sh \
          ghcr.io/ethstorage/es-node:v0.2.6

```
#### Start es-node

> ℹ️ _**Note:**_ Append `--miner.enabled=false` to the following commands if the miner account is not yet whitelisted.

* Mainnet

```sh
docker run --name es -d \
          -v ./es-data:/es-node/es-data \
          -v ./zkey:/es-node/build/bin/snark_lib/zkey \
          -e ES_NODE_STORAGE_MINER=<miner> \
          -e ES_NODE_SIGNER_PRIVATE_KEY=<private_key> \
          -p 9545:9545 \
          -p 9222:9222 \
          -p 30305:30305/udp \
          --entrypoint /es-node/run-mainnet.sh \
          ghcr.io/ethstorage/es-node:v0.2.6 \
          --l1.rpc <el_rpc> \
          --l1.beacon <cl_rpc>
```
* Sepolia

```sh
docker run --name es -d \
          -v ./es-data:/es-node/es-data \
          -v ./zkey:/es-node/build/bin/snark_lib/zkey \
          -e ES_NODE_STORAGE_MINER=<miner> \
          -e ES_NODE_SIGNER_PRIVATE_KEY=<private_key> \
          -p 9545:9545 \
          -p 9222:9222 \
          -p 30305:30305/udp \
          --entrypoint /es-node/run.sh \
          ghcr.io/ethstorage/es-node:v0.2.6 \
          --l1.rpc <el_rpc> \
          --l1.beacon <cl_rpc>
```
* QuarkChain L2

```sh
docker run --name es -d \
          -v ./es-data:/es-node/es-data \
          -v ./zkey:/es-node/build/bin/snark_lib/zkey \
          -e ES_NODE_STORAGE_MINER=<miner> \
          -e ES_NODE_SIGNER_PRIVATE_KEY=<private_key> \
          -p 9545:9545 \
          -p 9222:9222 \
          -p 30305:30305/udp \
          --entrypoint /es-node/run-l2.sh \
          ghcr.io/ethstorage/es-node:v0.2.6
```

After launch, you can check docker logs using the following command:

```sh
docker logs -f es 
```

#### Mount data location using Docker volume option

Docker volumes (-v) are a mechanism for storing data outside containers. In the above `docker run` command, you have the flexibility to modify the default data file location (`./es-data`) on your host machine, ensuring that the disk space requirements are fulfilled. For example:

```sh
docker run --name es  -d  \
          -v /host/path/with/large/space:/es-node/es-data \
          ...
```

> ℹ️ _**Note:**_ The absolute host path does not function well on Windows, for more details please refer [here](storage-provider-faq.md#when-running-es-node-in-docker-on-windows-i-want-to-store-data-files-on-a-disk-other-than-c-how-can-i-achieve-this).

### From source code

You will need to [install Go](tutorials.md#install-go) to build es-node from source code.

Just like running a pre-built, if you plan to utilize the default zkSNARK implementation, ensure that you have installed [Node.js](tutorials.md#install-node.js) and [snarkjs](tutorials.md#install-snarkjs).

If you intend to build es-node on Ubuntu and use `rapidsnark` as zkSNARK implementation, be sure to [verify some dependencies](tutorials.md#install-rapidsnark-dependencies).

#### Download source code and switch to the latest release branch

```sh
git clone https://github.com/ethstorage/es-node.git
cd es-node
git checkout v0.2.6
```

#### Build es-node

```sh
make
```

#### Initialize es-node

* Mainnet

```sh
env ES_NODE_STORAGE_MINER=<miner> ./init-mainnet.sh --l1.rpc <el_rpc>
```
* Sepolia

```sh
env ES_NODE_STORAGE_MINER=<miner> ./init.sh --l1.rpc <el_rpc>
```
* QuarkChain L2

```sh
env ES_NODE_STORAGE_MINER=<miner> ./init-l2.sh
```

#### Start es-node

> ℹ️ _**Note:**_ Append `--miner.enabled=false` to the following commands if the miner account is not yet whitelisted.

* Mainnet

```sh
env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> ./run-mainnet.sh --l1.rpc <el_rpc> --l1.beacon <cl_rpc>
```

* Sepolia

```sh
env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> ./run.sh --l1.rpc <el_rpc> --l1.beacon <cl_rpc>
```

* QuarkChain L2

```sh
env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> ./run-l2.sh
```

## Applying for Ethereum API 

> ℹ️ _**Note:**_ The steps below describe applying for Sepolia endpoints. For Ethereum Mainnet, the process is similar—choose Mainnet where applicable and follow provider-specific details.

### Applying for a free Sepolia execution layer endpoint from BlockPI

Go to the [BlockPI](https://blockpi.io/) website, click `Get Started`. After signing in, you will get your `Free Package Gift`. Click `Generate API Key`, and remember to select `Sepolia`, and you will get your API endpoint.

Finally, access the detailed page of your API endpoint and activate the `Archive Mode` under the `Advanced Features` section.

> ℹ️ _**Note:**_ The free plan of BlockPI provides sufficient usage as execution layer RPC for es-node, but it cannot be used as a beacon endpoint.

### Applying for a free Sepolia beacon endpoint from QuickNode

Go to the [QuickNode](https://www.quicknode.com/) website, click `Get started for free`. After signing in, you can create an endpoint. Remember to select `Ethereum` and `Sepolia` to continue. In the `Compliance & Safety` category, select `Endpoint Armor`, and select the free plan. After completing the required information, you will receive the endpoint along with an API key.

> ℹ️ _**Note:**_ The free plan of QuickNode is adequate for use as a beacon endpoint (\<cl\_rpc>) for running es-node, but is NOT sufficient as execution layer endpoint.

## Install dependencies

> ℹ️ _**Note:**_ Not all steps in this section are required; they depend on your [_choice_](tutorials.md#options-for-running-es-node).

### Install Go

Download a stable Go release, e.g., go1.23.7.

```sh
curl -OL https://golang.org/dl/go1.23.7.linux-amd64.tar.gz
```

Extract and install

```sh
tar -C /usr/local -xf go1.23.7.linux-amd64.tar.gz
```

Update `$PATH`

```sh
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

### Install RapidSNARK dependencies

Check if `build-essential` and `libomp-dev packages` are installed on your Ubuntu system:

```sh
dpkg -l | grep build-essential
dpkg -l | grep libomp-dev
```

Install the build-essential and libomp-dev packages if no information printed:

```sh
apt update
apt install build-essential
apt install libomp-dev
```

### Install libc6\_2.35

This installation is intended for scenarios where you encounter errors like this while running the pre-built es-node on Ubuntu 20.04:

```sh
/lib/x86_64-linux-gnu/libc.so.6: version 'glibc_2.32' not found
/lib/x86_64-linux-gnu/libc.so.6: version 'glibc_2.34' not found
```

To prevent the error, add the following line to your /etc/apt/sources.list:

```sh
deb http://security.ubuntu.com/ubuntu jammy-security main 
```

Next, install libc6\_2.35 by running the following commands:

```sh
apt update
apt install -y libc6
```

## Check the status after launching the es-node

It's important to monitor the node closely until it successfully submits its first storage proof. Typically, the process encompasses three main stages.

### Data sync phase

The es-node will synchronize data from other peers in the network. You can check from the console the number of peers the node is connected to and, more importantly, the estimated syncing time.

During this phase, the CPUs are expected to be fully occupied for data processing. If not, please refer to [the FAQ](storage-provider-faq.md#how-to-tune-the-performance-of-syncing) for performance tuning on this area.

A typical log entry in this phase appears as follows:

```
INFO [01-18|09:13:32.564] Storage sync in progress progress=85.50% peerCount=2 syncTasksRemain=1@0 blobsSynced=1@128.00KiB blobsToSync=0 fillTasksRemain=30 emptyFilled=3,586,176@437.77GiB emptyToFill=608,127   timeUsed=1h48m7.556s  eta=18m20.127s

```
And the sync phase ends with a log entry that reads `Storage sync done`.

> ℹ️ _**Note:**_ If mining is disabled (`--miner.enabled=false`), the node enters a stable state once sync completes.  The following repeating logs indicate the node is launched successfully:
```
INFO [10-15|08:07:53.121] Scanner stats                            localKvs=shard0[0-7583] localKvsCount=7584
INFO [10-15|08:08:52.983] P2P Summary                              activePeers=8 inbound=0 outbound=8
INFO [10-15|08:08:52.991] Scanner stats                            localKvs=shard0[0-7583] localKvsCount=7584
INFO [10-15|08:09:52.983] P2P Summary                              activePeers=8 inbound=0 outbound=8
```

### Sampling phase

Once data synchronization is complete, the es-node will enter the sampling phase, also known as mining.

A typical log entry of sampling during a slot looks like this:

```
INFO [01-19|05:02:23.210] Mining info retrieved                    shard=0 LastMineTime=1,705,634,628 Difficulty=4,718,592,000 proofsSubmitted=6
INFO [01-19|05:02:23.210] Mining tasks assigned                    shard=0 threads=64 block=10,397,712 nonces=1,048,576
INFO [01-19|05:02:26.050] Sampling done with all nonces            samplingTime=2.8s shard=0 block=10,397,712

```

When you see "Sampling done with all nonces", it indicates that your node has successfully completed all the sampling tasks within a slot. The "samplingTime" value informs you of the duration, in seconds, it took to complete the sampling process.

If the es-node doesn't have enough time to complete sampling within a slot, the log will display "Mining tasks timed out". For further actions, please refer to [the FAQ](storage-provider-faq.md#what-can-i-do-about-mining-tasks-timed-out).

Once data sync is finished, you will receive an email notification indicating which shard has been started mining, if email notifications have been [enabled](email-notification-guide-for-proof-submission.md) on your es-node.

### Proof submission phase

Once the es-node calculates a valid storage proof, it will submit the proof to the EthStorage contract and receive the rewards.

A typical log entry of submitting proof looks like this:

```
INFO [01-19|05:02:23.210] Calculated a valid hash                  shard=0 thread=3 block=5,437,371 nonce=58101
INFO [01-19|05:05:23.210] Mining transaction confirmed"            txHash=0x7afa13e5211c403a7024bdf0a6880203d54698355679be9aab1aa0ecef78eecd
INFO [01-19|05:05:23.210] Mining transaction success! √            miner=0xBB9D13efa21c0a053eCFE622e2AbfAF0D7573f50
```

Once you see this log, congratulations on completing the entire process as a storage provider. You can also check how many storage proofs you've submitted, your ETH profit, and your ranking on the [dashboard](https://grafana.ethstorage.io).

Once a valid proof has been submitted, you will receive an email notification with the detailed information, if email notifications have been [enabled](email-notification-guide-for-proof-submission.md) on your es-node.

## Advanced Features

### About `run*.sh` and `init*.sh`

The `run*.sh` scripts serves as the entry point for launching the es-node with predefined parameters. By default, mining is enabled through the `--miner.enabled` flag in `run*.sh`, which implies that you assume the role of a storage provider upon starting an es-node with the default settings.

However, before the es-node can be successfully launched, you must execute `init*.sh` first. The primary function of this script is to verify the system environment, download and install dependencies, and initialize the data files in preparation for mining.

For specific usage and examples of the two scripts, refer to the steps outlined in [Options for running es-node](tutorials.md#options-for-running-es-node).


> ℹ️ _**Note:**_ By default, `init.sh` and `run.sh` target Sepolia; use `init-mainnet.sh`/`run-mainnet.sh` for Ethereum Mainnet and `init-l2.sh`/`run-l2.sh` for QuarkChain L2.

This applies to every shell command throughout the rest of this guide.

> ℹ️ _**Note:**_ Some of the flags/parameters used in the shell scripts are supposed to change over time. Refer to [_configuration_](configuration.md) for a full list.

### Mining multiple shards

By default, only the first shard (shard 0) is mined using the default options in the scripts, but you have the choice to initialize and run your es-node in order to mine multiple selected shards by using additional options.

Take Sepolia network as example, the flag `--shard_index` can be utilized multiple times with `init.sh` to generate data files for multiple shards on the es-node. For example,

```
env ES_NODE_STORAGE_MINER=0x123...ab ./init.sh --shard_index 1 --shard_index 2
```

Please take note of the following:

* The shard files will be generated in the `./es-data` directory with the naming convention `shard-$(shard_index).dat` by default settings in `init.sh`.
* A shard will be omitted if its corresponding data file already exists.
* `shard-0.dat` will be tried to create if no `--shard_index` is specified.
* For Mainnet, use `init-mainnet.sh` to replace `init.sh`; for QuarkChain L2, use `init-l2.sh`.

After initialization in this way, the `run.sh` script will attempt to operate on data files located in `./es-data/shard-*.dat`. If you have relocated these data files or added additional files in another location, you can specify them using the `--storage.files` options repeatedly following `./run.sh`.

### About the option of zk prover implementation

The `--miner.zk-prover-impl` flag specifies the type of zkSNARK implementation. Its default value is `1`, indicating the generation of zk proofs using snarkjs. The option `2` means to utilize go-rapidsnark. Since `--miner.zk-prover-impl` interacts closely with the environment, it is crucial to use the same configuration when running both `init.sh` and `run.sh`.

> ℹ️ _**Note:**_ If you have to run an es-node pre-built with `--miner.zk-prover-impl 2` on Ubuntu 20.04, you will need to [install extra packages](tutorials.md#install-libc6_235).
