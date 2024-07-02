
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

const deploy = async () => {
    const ETH_STORAGE_ADDRESS = "0x804C520d3c084C805E37A35E90057Ac32831F96f";
    const rpc = "https://rpc.sepolia.org";
    const privateKey = "0x...";
 
    const ethStorage = new EthStorage(rpc, privateKey);
    await ethStorage.deploy(ETH_STORAGE_ADDRESS);
    // FlatDirectory address: 0x37df32c7a3c30d352453dadacc838461d8629016
}
```

If you are on the 'Sepolia' network, you can do the following:
```js
const deploySepolia = async () => {
    const rpc = "https://rpc.sepolia.org";
    const privateKey = "0x...";

    const ethStorage = new EthStorage(rpc, privateKey);
    await ethStorage.deploySepolia();
    // FlatDirectory address: 0x37df32c7a3c30d352453dadacc838461d8629016
}
```

If you already have a `FlatDirectory` contract, you can set it.
```js
const {EthStorage} = require("ethstorage-sdk");

const create = async () => {
    const rpc = "https://rpc.sepolia.org";
    const privateKey = "0x...";
    const flatDirectory = "0x37df32c7a3c30d352453dadacc838461d8629016";
 
    const ethStorage = new EthStorage(rpc, privateKey, flatDirectory);
}
```


### 2.2: Upload Files To FlatDirectory

In this section, you will upload some files into the `FlatDirectory` that you just created.

Get EthStorage
```js
const {EthStorage} = require("ethstorage-sdk");

const getEthStorage = () => {
    const rpc = "https://rpc.sepolia.org";
    const privateKey = "0x...";
    const flatDirectory = "0x37df32c7a3c30d352453dadacc838461d8629016";
    const ethStorage = new EthStorage(rpc, privateKey, flatDirectory);
    return ethStorage;
}
```

You can set the file or folder path, and if it is a browser environment, you can also set the file object.
```js
const upload = async () => {
    const pathOrFile = "/users/dist/test.txt";
    const ethStorage = getEthStorage();
    await ethStorage.upload(pathOrFile);
    // hash: 0x753...45836
}
```

If you want to upload data, use 'uploadData'
```js
const upload = async () => {
    const fileName = "test.txt";
    const filePath = "/usr/dist/test.txt";
    const data = fs.readFileSync(filePath);

    const ethStorage = getEthStorage();
    await ethStorage.uploadData(fileName, data);
}
```

### 2.3: Download Files From Flat Directory

In this section, you will download files from the ethstorage network.

```js
const download = async (fileName) => {
    const ethStorage = getEthStorage();
    // Note: This should be ethstorage rpc, not eth rpc
    const ethStorageRpc = "https://ethstorage.rpc.io";
    const data = await ethStorage.download(fileName, ethStorageRpc);
    // You can save the data as a file
}
```

or
```js
const { Download } = require("ethstorage-sdk")

const download = async (fileName) => {
    // Note: This should be ethstorage rpc, not eth rpc
    const ethStorageRpc = "https://ethstorage.rpc.io";
    const flatDirectory = "0x37df32c7a3c30d352453dadacc838461d8629016";

    const data = await Download(ethstorageRpc, flatDirectory, fileName);
    // You can save the data as a file
}
```
