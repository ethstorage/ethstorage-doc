# Upload Your File/Folder with ethfs-cli

## Introduction

In this tutorial, we will demonstrate how to upload files or folders to an EVM compatiable chain using the [ethfs-cli](https://github.com/ethstorage/ethfs-cli/) tool. Find the list of the currently supported chains [here](https://github.com/ethstorage/ethfs-cli/?tab=readme-ov-file#supported-networks).

For the content to be uploaded, let's assume that

* there is a folder to be uploaded; (e.g., `dist`)
* there are two files in the folder. (e.g., `hello.txt` and `img/1.jpeg`)

<figure><img src="broken-reference" alt=""><figcaption></figcaption></figure>

## Step 1: Install `ethfs-cli`

If you have not already done so, you can install `ethfs-cli` using the following command:

```
npm i -g ethfs-cli
```

## Step 2: Create a `FlatDirectory` Contract

A [FlatDirectory](https://docs.web3url.io/advanced-topics/flatdirectory) contract serves as a container that needs to be created before uploading files. 

The following command creates a `FlatDirectory` on the default chain (**Ethereum Mainnet**) using the private key `0x112233...`

```
$ ethfs-cli create -p 0x112233...
```

If you intend to create it on other chains that support EVM, you need to add the chain ID using the `-c` flag, for example, `11155111` for **Sepolia**.

```
$ ethfs-cli create -p 0x112233... -c 11155111
```

You will get a `FlatDirectory` address after the transaction is confirmed:

```
$ ethfs-cli create -p 0x112233... -c 11155111
chainId = 11155111
providerUrl = https://rpc.sepolia.org
FlatDirectory: Address is 0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd
```

## Step 3: Upload Files

In this section, you will upload the files/folder into the `FlatDirectory` that you just created using the following command:

```
ethfs-cli upload -f <directory|file> -a <address> -c <chain-id> -p <private-key> -t <upload-type>
```
Notice that you have the option to specify the file upload type: `calldata`, `blob`.  The default type is `blob` which requires network support for EIP-4844.

For example:

```
$ ethfs-cli upload -f dist -a 0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd -c 11155111 -p 0x112233... -t blob
providerUrl = https://rpc.sepolia.org
chainId = 11155111
address: 0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd


FlatDirectory: The transaction hash for chunk 0 is 0x809411aeb708023a33dadf17791d994dc3b4b2db1a6bbd36792bbedb68646978 img/1.jpeg
FlatDirectory: Chunks 0 have been uploaded hello.txt
FlatDirectory: The transaction hash for chunk 0 is 0x7cea7ea7e4898e03bee4fbc031799689dbe215bd6dd36733721150a099680be7 hello.txt
FlatDirectory: Chunks 0 have been uploaded img/1.jpeg

Total File Count: 2
Total Upload Chunk Count: 2
Total Upload Data size: 52.6513671875 KB
Total Storage Cost: 0.001492087764775451 ETH
```

## Step 4: Download Your File!

Now you should be able to download the file you just uploaded.

```
ethfs-cli download -a <address> -c <chain-id> -f <file>
```

Run the command to download the file.

```
ethfs-cli download -a 0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd -c 11155111 -f img/1.jpeg
```

Now, your file has been saved locally.

## Step 5: Access Your File via `web3://` 

Of course, you can also easily access the file you just uploaded using the web3:// protocol. For example,

[web3://0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd:3333/hello.txt](https://0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd.3333.w3link.io/hello.txt)

or,

[web3://0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd:3333/img/1.jpeg](https://0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd.3333.w3link.io/img/1.jpeg)

To gain further insights into `web3://` protocol, you can visit [web3url.io](https://web3url.io).

## Optional: Using Your Own RPC Endpoint

You can also specify your own RPC for better performance by `-r` flag in the above steps. 

For example when create `FlatDirectory` contract:

```
$ ethfs-cli create -p 0x112233... -c 11155111 -r http://...rpc.io
```

When uploading files:

```
ethfs-cli upload -f /Users/.../dist -a 0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd -c 11155111 -p 0x112233... -r https://...rpc.io
```
