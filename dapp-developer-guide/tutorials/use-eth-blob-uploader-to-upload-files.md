# Use eth-blob-uploader to upload files

## Introduction

In this tutorial, we will demonstrate how to upload an image using the [eth-blob-uploader](https://github.com/ethstorage/eth-blob-uploader) tool. The image will be uploaded to the blockchain using EIP-4844 blob transaction.

## Installation
 
If you have not already done so, you can install `eth-blob-uploader` using the following command:

```
npm i -g eth-blob-uploader
```

## Basic Operation

Basically, you can upload the file using the following syntax:

```
eth-blob-uploader -r <rpc> -p <private-key> -f <file-path> -t <to-address>
```

In the following example, you will upload the file `img.jpeg` to the address `0xaa...` using the private key `0x...a7` via RPC `http://...rpc.io`.

```
eth-blob-uploader -r http://...rpc.io -p 0x...a7 -f /Users/test/img.jpeg -t 0xaa...
```

## More Options

If the `to-address` is a smart contract, you will need to pass in calldata using `-d`.

```
// eth-blob-uploader -r <rpc> -p <private-key> -f <file-path> -t <to-address> -d [data]
eth-blob-uploader -r http://...rpc.io -p 0x...a7 -f /Users/test/img.jpeg -t 0xbb... -d 0x...11
```

You can specify the number of blobs that come with a transaction using `-c`, with default value set to `3`.

```
// eth-blob-uploader -r <rpc> -p <private-key> -f <file-path>  -t <to-address> -c [blob-counts]
eth-blob-uploader -r https://...rpc.io -p 0x...a7 -f /Users/test/img.jpeg -t 0xaa... -c 2
```

You can also override the pending transaction by passing in parameters such as nonce(`-n`) and gas(`-g` for transaction gas and `-b` for blob gas).

```
// eth-blob-uploader -r <rpc> -p <private-key> -f <file-path> -t <to-address> -n [nonce] -g [gas] -b [blob-gas]
eth-blob-uploader -r http://...rpc.io -p 0x...a7 -f /Users/test/img.jpeg -t 0xaa... -n 1 -g 1000000000 -b 2000000000
```

