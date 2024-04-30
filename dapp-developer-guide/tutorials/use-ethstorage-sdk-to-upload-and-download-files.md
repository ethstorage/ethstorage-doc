
# Use ethstorage-sdk to Upload and Download Files

## Introduction

In this tutorial, we will demonstrate how to upload and download files using the [**ethstorage-sdk**](https://github.com/ethstorage/ethstorage-sdk) tool.

The `ethstorage-sdk` provides APIs for file upload, download, and management.

## Step 1: Install ethstorage-sdk

You can install `ethstorage-sdk` by the following command:

```sh
npm i ethstorage-sdk
```

## Step 2: Manage Files

### 2.1 Create Flat Directory

In this section, you will create a `FlatDirectory` contract for managing files.

```js
const {EthStorage} = require("ethstorage-sdk");

const create = async () => {
    const ETH_STORAGE_ADDRESS = "0x804C520d3c084C805E37A35E90057Ac32831F96f";
    const rpc = "https://rpc.sepolia.org";
    const privateKey = "0x...";
 
    const ethStorage = new EthStorage(rpc, privateKey);
    await ethStorage.deploy(ETH_STORAGE_ADDRESS);
    // FlatDirectory address: 0x37df32c7a3c30d352453dadacc838461d8629016

    or

    // If you are on the Sepola network, you can do the following:
    await ethStorage.deploySepolia();
}
```

If you already have a `FlatDirectory` contract, you can set it directly.

```js
const {EthStorage} = require("ethstorage-sdk");

const create = async () => {
    const rpc = "https://rpc.sepolia.org";
    const privateKey = "0x...";
    const flatDirectory = "0x37df32c7a3c30d352453dadacc838461d8629016";
 
    const ethStorage = new EthStorage(rpc, privateKey, flatDirectory);
}
```


### 2.2: Upload Files To Flat Directory

In this section, you will upload some files into the `FlatDirectory` that you just created.

```js
const {EthStorage} = require("ethstorage-sdk");

const upload = async (filePath) => {
    // You can set the path of a file or folder. If using a browser, you can specify the file object selected by the 'input'.
    await ethStorage.upload(pathOrFile);
    // hash: 0x753...45836

    or

    // If you want to upload data, use 'uploadData'.
    const data = fs.readFileSync(filePath);
    await ethStorage.uploadData(fileName, data);
}
```


### 2.3: Download Files From Flat Directory

In this section, you will download files from the ethstorage network.

```js
const {EthStorage} = require("ethstorage-sdk");

const download = async (fileName) => {
    // Note: This should be ethstorage rpc, not eth rpc
    const ethStorageRpc = "https://ethstorage.rpc.io";
    let data = await ethStorage.download(fileName, ethStorageRpc);

    or

    const { Download } = require("ethstorage-sdk")
    const flatDirectory = "0x37df32c7a3c30d352453dadacc838461d8629016";
    data = await Download(ethstorageRpc, flatDirectory, fileName);

    // You can save the data as a file
}
```

