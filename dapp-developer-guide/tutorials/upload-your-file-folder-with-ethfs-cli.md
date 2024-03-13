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
FlatDirectory Address: 0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd
```

## Step 3: Upload Files

In this section, you will upload the files/folder into the `FlatDirectory` that you just created.

First of all, be noticed that the short name of the chain must be added before the contract address to identify the chain on which the `FlatDirectory` contract is located:

```
shortName:address
```

See more details about [the EIP-3770 address](https://eips.ethereum.org/EIPS/eip-3770). And you can find the short name of the supported chain [here](https://github.com/ethstorage/ethfs-cli/?tab=readme-ov-file#supported-networks).


For example, the address created above on the **Sepolia** network are spliced ​​as follows:

```
sep:0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd
```

Now you can use the following command to upload the folder:

```
ethfs-cli deploy -f <directory|file> -a <shortName:address> -p <private-key> -t <upload-type>
```
Notice that you have the option to specify the file upload type: `1` for calldata, `2` for EIP-4844 blob.  The default type is `2` which requires network support for EIP-4844.

For example:

```
$ ethfs-cli deploy -f dist -a sep:0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd -p 0x112233... -t 2
providerUrl = https://rpc.sepolia.org
chainId = 11155111
address: 0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd

Start upload File.......

img/1.jpeg, chunkId: 0
hello.txt, chunkId: 0
Transaction Id: 0x809411aeb708023a33dadf17791d994dc3b4b2db1a6bbd36792bbedb68646978
File img/1.jpeg chunkId: 0 uploaded!
Transaction Id: 0x7cea7ea7e4898e03bee4fbc031799689dbe215bd6dd36733721150a099680be7
File hello.txt chunkId: 0 uploaded!

Total Cost: 0.001492087764775451 ETH
Total File Count: 2
Total File Size: 52.6513671875 KB
```

## Step 4: Download Your File!

Now you should be able to download the file you just uploaded.

```
ethfs-cli download -a <shortName:address> -f <file>
```

Run the command to download the file.

```
ethfs-cli download -a sep:0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd -f 1.jpeg
```

Now, your file has been saved locally.


## Optional: Using Your Own RPC Endpoint

You can also specify your own RPC for better performance by `-r` flag in the above steps. 

For example when create `FlatDirectory` contract:

```
$ ethfs-cli create -p 0x112233... -c 11155111 -r http://...rpc.io
```

When uploading files:

```
ethfs-cli deploy -f /Users/.../dist -a sep:0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd -p 0x112233... -r https://...rpc.io
```