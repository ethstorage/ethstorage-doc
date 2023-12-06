---
description: Join the EthStorage network and start mining ASAP!
---

# Run a Node

This tutorial serves as a general guide to initiate an es-node instance and connect it to the existing EthStorage network. For detailed and updated instructions, it is recommended to access [this link](https://github.com/ethstorage/es-node#es-node).

## Minimum hardware requirements

This is the minimum hardware requirement that an es-node can run. Note that 1.2TB of free disk space can only support one data shard to be stored. You may need more disk space to store multiple shards according to your requirements.

* CPU with 2+ cores
* 4GB RAM
* 1.2TB free disk space dedicated to data storage
* 8 MBit/sec download Internet service

_Note: A Ubuntu 20.04 system is recommended as the following steps have been tested with it._

## Step 1. Prepare miner and signer account

It is suggested to prepare two Ethereum accounts to run an es-node as a storage provider, one of which needs to have some ETH balance to be used as a transaction signer.

Remember to use the signer's private key (with ETH balance) to replace `<private_key>` in the following steps. And use the other address to replace `<miner>`.

_Note: An alternative option to sign transactions instead of passing a private key around would be using a remote signer such as `clef`._

## Step 2. Download source code

```sh
# download source code
git clone git@github.com:ethstorage/es-node.git

# go to the repo
cd es-node
```

## Step 3. Run es-node

You can choose how to run es-node according to your current system environment.

### Option 1: With Docker compose

If you have Docker version 24.0.5 or above installed, simply run:

```sh
env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> docker compose up 
```

### Option 2: With Docker in the background

If you have Docker version 24.0.5 or above installed, and prefer to keep all the logs:

```sh
env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> ./run-docker.sh

# check logs
docker logs -f es 
```

### Option 3: Without Docker

If Docker is not installed or you prefer to run an executable binary directly, here is a detailed tutorial for you to get everything ready.

If you may already have go 1.20.* and node 16+ installed, just ignore the corresponding steps.

#### 1. Install go 1.20.* (e.g. v1.20.10)

Download a stable go release

```sh
curl -OL https://golang.org/dl/go1.20.10.linux-amd64.tar.gz
```

Extract and install

```sh
tar -C /usr/local -xf go1.20.10.linux-amd64.tar.gz
```

Edit `~/.profile` and add the following line to the end of it.

```
export PATH=$PATH:/usr/local/go/bin
```

Next, refresh your profile by running the following command:

```sh
source ~/.profile
```

#### 2. Install node 16+ (e.g. v20.\*)

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

Install node using nvm

```sh
nvm install 20
```

Activate the node version

```sh
nvm use 20
```

#### 3. Install `snarkjs`

```sh
npm install -g snarkjs@0.7.0
```

#### 4. Build es-node

```sh
cd cmd/es-node && go build && cd ../..
```

#### 5. Start es-node

```sh
chmod +x run.sh && env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> ./run.sh
```
