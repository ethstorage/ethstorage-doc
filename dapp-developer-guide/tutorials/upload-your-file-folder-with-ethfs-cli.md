# Upload Your File/Folder with ethfs-cli

## Introduction
In this tutorial, we will demonstrate how to use the [ethfs-cli](https://github.com/ethstorage/ethfs-cli/) tool to upload files or folders to EVM-compatible chains such as Sepolia or [Super World Computer](https://quarkchain.io) beta testnet.

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
FlatDirectory: Address is 0x1B7D3660fB596A9FdF0e6fd7566A4190A4d00E3a
```

### Example: SWC Beta

```bash
ethfs-cli create -p 0x112233... -c 3335
```

You will get a `FlatDirectory` address after the transaction is confirmed on SWC beta testnet:

```bash
FlatDirectory: Address is 0xab351F35B82B20C1a253ae16523c5E2D60B56D6E
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
ethfs-cli upload -f dist -a 0x1B7D3660fB596A9FdF0e6fd7566A4190A4d00E3a -c 11155111 -p 0x112233... -t blob
```
Example log:
```log
providerUrl = https://rpc.sepolia.org
chainId = 11155111
address: 0x1B7D3660fB596A9FdF0e6fd7566A4190A4d00E3a


FlatDirectory: The transaction hash for chunk 0 is 0x809411aeb708023a33dadf17791d994dc3b4b2db1a6bbd36792bbedb68646978 img/1.jpeg
FlatDirectory: Chunks 0 have been uploaded hello.txt
FlatDirectory: The transaction hash for chunk 0 is 0x7cea7ea7e4898e03bee4fbc031799689dbe215bd6dd36733721150a099680be7 hello.txt
FlatDirectory: Chunks 0 have been uploaded img/1.jpeg

Total File Count: 2
Total Upload Chunk Count: 2
Total Upload Data size: 52.6513671875 KB
Total Storage Cost: 0.001492087764775451 ETH
```

### Example: SWC Beta

```bash
ethfs-cli upload -f dist -a 0xab351F35B82B20C1a253ae16523c5E2D60B56D6E -c 3335 -p 0x112233...
```
Example log:
```log
providerUrl = https://rpc.beta.testnet.l2.quarkchain.io:8545
chainId = 3335
address = 0xab351F35B82B20C1a253ae16523c5E2D60B56D6E 
threadPoolSize = 15 

FlatDirectory: The transaction hash for chunk 0 is 0x2526108470cb100837ac1a724df91c9ba3d1422fb2e45fec458cc3f566d5f210  hello.txt
FlatDirectory: Chunks 0 have been uploaded  hello.txt
FlatDirectory: The transaction hash for chunks 0,1,2 is 0xfec795480524f81964e62251e8ac7d0f0dc9ed8422bc96a300254377043d3721  img/1.jpeg
FlatDirectory: Chunks 0,1,2 have been uploaded  img/1.jpeg

Total File Count: 2
Total Upload Chunk Count: 4
Total Upload Data Size: 324.1015625 KB
Total Storage Cost: 2.259012840557991428 ETH
```
## Step 4: Download Your File!

Now you should be able to download the file you just uploaded.

### Command Syntax

```bash
ethfs-cli download -a <address> -c <chain-id> -f <file>
```

### Example: Sepolia

```bash
ethfs-cli download -a 0x1B7D3660fB596A9FdF0e6fd7566A4190A4d00E3a -c 11155111 -f img/1.jpeg
```

### Example: SWC Beta

```bash
ethfs-cli download -a 0xab351F35B82B20C1a253ae16523c5E2D60B56D6E -c 3335 -f img/1.jpeg
```

Now, your file has been saved locally.

## Step 5: Access Your File via `web3://` 

Of course, you can also easily access the file you just uploaded using the web3:// protocol. 

### Example: Sepolia

text:
[web3://0x1B7D3660fB596A9FdF0e6fd7566A4190A4d00E3a:3333/hello.txt](https://0x1B7D3660fB596A9FdF0e6fd7566A4190A4d00E3a.3333.w3link.io/hello.txt)

image:
[web3://0x1B7D3660fB596A9FdF0e6fd7566A4190A4d00E3a:3333/img/1.jpeg](https://0x1B7D3660fB596A9FdF0e6fd7566A4190A4d00E3a.3333.w3link.io/img/1.jpeg)

### Example: SWC Beta

text:
[web3://0xab351F35B82B20C1a253ae16523c5E2D60B56D6E:3337/hello.txt](https://0xab351F35B82B20C1a253ae16523c5E2D60B56D6E.3337.w3link.io/hello.txt)

image:
[web3://0xab351F35B82B20C1a253ae16523c5E2D60B56D6E:3337/img/1.jpeg](https://0xab351F35B82B20C1a253ae16523c5E2D60B56D6E.3337.w3link.io/img/1.jpeg)

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
ethfs-cli upload -f /Users/.../dist -a 0x1B7D3660fB596A9FdF0e6fd7566A4190A4d00E3a -c 11155111 -p 0x112233... -r https://...rpc.io
```
