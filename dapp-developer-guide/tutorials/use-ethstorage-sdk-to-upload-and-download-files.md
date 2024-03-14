
# Use ethstorage-sdk to Upload and Download Files

## Introduction

In this tutorial, we will demonstrate how to upload and download files using the [**ethstorage-sdk**](https://github.com/ethstorage/ethstorage-sdk) tool.

The `ethstorage-sdk` provides APIs for file upload, download, and management, which are built on `c-kzg` and, therefore, are not compatible with a browser environment.

## Step 1: Install ethstorage-sdk

You can install `ethstorage-sdk` by the following command:

```sh
npm i ethstorage-sdk
```

## Step 2: Upload Files


```js
const {BlobUploader, EncodeBlobs} = require("ethstorage-sdk");

const upload = async (filePath) => {
    const rpc = "https://rpc.sepolia.org";
    const privateKey = "0x...";
    const tx = {
      value: cost,
      to: to,
      data: data,
    };
    
    const content = fs.readFileSync(filePath);
    const blobs = EncodeBlobs(content);
    const blobUploader = new BlobUploader(rpc, privateKey);
    const hash = await blobUploader.sendTx(tx, [blobs[0], blobs[1]...) // max is 6
    // hash : 0x37df32c7a3c30d3...52453dadacc838461d8629016
}
```

## Step 3: Manage Files

### 3.1 Create Flat Directory

In this section, you will create a `FlatDirectory` contract for managing files.


```js
const {EthStorage} = require("ethstorage-sdk");

const create = async () => {
    const ETH_STORAGE_ADDRESS = "0x804C520d3c084C805E37A35E90057Ac32831F96f";
    const rpc = "https://rpc.sepolia.org";
    const privateKey = "0x...";
 
    const ethStorage = new EthStorage(rpc, privateKey);
    await ethStorage.deployBlobDirectory(ETH_STORAGE_ADDRESS);
    // flatDirectory address: 0x37df32c7a3c30d352453dadacc838461d8629016
}
```

### 3.2: Upload Files To Flat Directory

In this section, you will upload some files into the `FlatDirectory` that you just created.

```js
const {EthStorage} = require("ethstorage-sdk");

const upload = async (filePath) => {
    const rpc = "https://rpc.sepolia.org";
    const privateKey = "0x...";
 
    const ethStorage = new EthStorage(rpc, privateKey);
    await ethStorage.upload(filePath);
    // hash: 0x753...45836
}
```

### 3.3: Download Files From Flat Directory

In this section, you will download files from the ethstorage network.

```js
const {DownloadFile} = require("ethstorage-sdk");

const download = async (fileName) => {
    const flatDirectory = "0x37df32c7a3c30d352453dadacc838461d8629016";
    // Note: This should be ethstorage rpc, not eth rpc
    const rpc = "https://ethstorage.rpc.io";
 
    const data = await DownloadFile(rpc, flatDirectory, fileName);
    // You can save the data as a file
}
```

