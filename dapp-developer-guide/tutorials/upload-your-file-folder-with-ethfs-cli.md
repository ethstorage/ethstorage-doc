# Upload Your File/Folder with ethfs-cli

## Introduction
In this tutorial, we will demonstrate how to use the [ethfs-cli](https://github.com/ethstorage/ethfs-cli/) tool to upload files or folders to EVM-compatible chains such as Sepolia or [QuarkChain L2](https://quarkchain.io) testnet.

You can find the full list of currently supported chains [here](https://github.com/ethstorage/ethfs-cli/?tab=readme-ov-file#supported-networks). 

For the content to be uploaded, let's assume that

* there is a folder to be uploaded; (e.g., `dist`)
* there are two files in the folder. (e.g., `hello.txt` and `img/1.jpeg`)

## Step 1: Install `ethfs-cli`

If you have not already done so, you can install `ethfs-cli` using the following command:

```bash
npm i -g ethfs-cli
```
If you installed it a long time ago, please run the same command to update to the latest version.

## Step 2: Create a `FlatDirectory` Contract

A [FlatDirectory](https://docs.web3url.io/advanced-topics/flatdirectory) contract serves as a container that needs to be created before uploading files. 

### Command Syntax

```bash
ethfs-cli create -p <private-key> -c <chain-id>
```
This command creates a `FlatDirectory` on a specified blockchain. You need to provide a private key and the chain ID. 

If the part `-c <chain-id>` is omitted, **Ethereum Mainnet** will be specified by default.

### Example: Sepolia

```bash
ethfs-cli create -p 0x112233... -c 11155111
```

You will get a `FlatDirectory` address after the transaction is confirmed on Sepolia:

```bash
FlatDirectory: Address is 0x8FE13f6697B1A8c34460D0E1375bbD205834D208
```

### Example: QuarkChain L2

```bash
ethfs-cli create -p 0x112233... -c 110011
```

You will get a `FlatDirectory` address after the transaction is confirmed on QuarkChain L2 testnet:

```bash
FlatDirectory: Address is 0xbaD27E1893B5E096c8e638a781eC93d7413d10F3
```

## Step 3: Upload Files

In this section, you will upload the files/folder into the `FlatDirectory` that you just created.

### Command Syntax

```bash
ethfs-cli upload \
    -f <directory|file> \
    -a <address> \
    -c <chain-id> \
    -p <private-key> \
    -t <upload-type>
```
You need to provide the contract address (`-a <address>`), chain ID (`-c <chain-id>`), private key (`-p <private-key>`), and the type of upload (`-t <upload-type>`).

Notice that you have 2 options to specify the file upload type: `calldata`, `blob`.  The default type is `blob` which requires network support for EIP-4844.

If the part `-c <chain-id>` is omitted, **Ethereum Mainnet** will be specified by default.

### Example: Sepolia

```bash
ethfs-cli upload -f dist -a 0x8FE13f6697B1A8c34460D0E1375bbD205834D208 -c 11155111 -p 0x112233... -t blob
```
Example log:
```log
providerUrl = https://rpc.sepolia.org
chainId = 11155111
address: 0x8FE13f6697B1A8c34460D0E1375bbD205834D208


FlatDirectory: The transaction hash for chunk 0 is 0x809411aeb708023a33dadf17791d994dc3b4b2db1a6bbd36792bbedb68646978 img/1.jpeg
FlatDirectory: Chunks 0 have been uploaded hello.txt
FlatDirectory: The transaction hash for chunk 0 is 0x7cea7ea7e4898e03bee4fbc031799689dbe215bd6dd36733721150a099680be7 hello.txt
FlatDirectory: Chunks 0 have been uploaded img/1.jpeg

Total File Count: 2
Total Upload Chunk Count: 2
Total Upload Data size: 52.6513671875 KB
Total Storage Cost: 0.001492087764775451 ETH
```

### Example: QuarkChain L2

```bash
ethfs-cli upload -f dist -a 0xbaD27E1893B5E096c8e638a781eC93d7413d10F3 -c 110011 -p 0x112233...
```
Example log:
```log
providerUrl =  https://rpc.delta.testnet.l2.quarkchain.io:8545
chainId = 110011
address = 0xbaD27E1893B5E096c8e638a781eC93d7413d10F3 
threadPoolSize = 15 

FlatDirectory: Transaction hash: 0x87f4b9193a4dfc5b99425e3817f7f40162fea7440929ab152b8eece68bae513d for chunk(s) 0. (Key: hello.txt)
FlatDirectory: Transaction hash: 0xec6ce890f7e4e69e0dd371ad031023a15e6ef13fc0752e944bd95d3ab3e01b6a for chunk(s) 0. (Key: img/1.jpeg)
FlatDirectory: Chunks 0 have been uploaded for hello.txt.
FlatDirectory: Chunks 0 have been uploaded for img/1.jpeg.

Total File Count: 2
Total Upload Chunk Count: 2
Total data uploaded: 22.953125 KB
Total cost: 0.005524459292796327 ETH
```
## Step 4: Download Your File!

Now you should be able to download the file you just uploaded.

### Command Syntax

```bash
ethfs-cli download -a <address> -c <chain-id> -f <file>
```

### Example: Sepolia

```bash
ethfs-cli download -a 0x8FE13f6697B1A8c34460D0E1375bbD205834D208 -c 11155111 -f img/1.jpeg
```

### Example: QuarkChain L2

```bash
ethfs-cli download -a 0xbaD27E1893B5E096c8e638a781eC93d7413d10F3 -c 110011 -f img/1.jpeg
```

Now, your file has been saved locally.

## Step 5: Access Your File via `web3://` 

Of course, you can also easily access the file you just uploaded using the web3:// protocol. 

### Example: Sepolia

text:
[web3://0x8FE13f6697B1A8c34460D0E1375bbD205834D208:3333/hello.txt](https://0x8FE13f6697B1A8c34460D0E1375bbD205834D208.3333.w3link.io/hello.txt)

image:
[web3://0x8FE13f6697B1A8c34460D0E1375bbD205834D208:3333/img/1.jpeg](https://0x8FE13f6697B1A8c34460D0E1375bbD205834D208.3333.w3link.io/img/1.jpeg)

### Example: QuarkChain L2

text:
[web3://0xbaD27E1893B5E096c8e638a781eC93d7413d10F3:110011/hello.txt](https://0xbaD27E1893B5E096c8e638a781eC93d7413d10F3.110011.w3link.io/hello.txt)

image:
[web3://0xbaD27E1893B5E096c8e638a781eC93d7413d10F3:110011/img/1.jpeg](https://0xbaD27E1893B5E096c8e638a781eC93d7413d10F3.110011.w3link.io/img/1.jpeg)

**Note**: In the above URLs, you may need to specify a different chain ID than the one used in the `ethfs-cli` commands. This distinct chain ID is necessary for identifying the EthStorage network responsible for storing the files.

To gain further insights into `web3://` protocol, you can visit [web3url.io](https://web3url.io).

## Optional: Using Your Own RPC Endpoint

You can also specify your own RPC for better performance by `-r` flag in the above steps. 

For example when create `FlatDirectory` contract:

```bash
ethfs-cli create -p 0x112233... -c 11155111 -r http://...rpc.io
```

When uploading files:

```bash
ethfs-cli upload -f /Users/.../dist -a 0x8FE13f6697B1A8c34460D0E1375bbD205834D208 -c 11155111 -p 0x112233... -r https://...rpc.io
```
