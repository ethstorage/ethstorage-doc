
# Use ethstorage-sdk to Upload and Download Files

## Introduction

In this tutorial, we will demonstrate how to upload and download files using the [**ethstorage-sdk**](https://github.com/ethstorage/ethstorage-sdk) tool.

The `ethstorage-sdk` provides APIs for file upload, download, and management.

In the following code examples, [Super World Computer](https://quarkchain.io) beta testnet is used by default. 
You can easily switch to other chains by specify a different RPC endpoint, such as "https://rpc.sepolia.org" for Sepolia.

## Step 1: Install ethstorage-sdk

You can install `ethstorage-sdk` by the following command:

```sh
npm i ethstorage-sdk
```

## Step 2: Manage Files

### 2.1 Create FlatDirectory

In this section, you will create a `FlatDirectory` contract for managing files.

```js
const { FlatDirectory } = require("ethstorage-sdk");

const rpc = "https://rpc.beta.testnet.l2.quarkchain.io:8545";
// For Sepolia:
// const rpc = "https://rpc.sepolia.org";
const privateKey = "0x...";

const flatDirectory = await FlatDirectory.create({
    rpc: rpc,
    privateKey: privateKey,
});
const contracAddress = await flatDirectory.deploy();
console.log(`FlatDirectory address: ${contracAddress}.`);
// FlatDirectory address: 0x37df32c7a3c30d352453dadacc838461d8629016
```

If you already have a `FlatDirectory` contract, you can set it.
```js
const address = "0x37df32c7a3c30d352453dadacc838461d8629016"; // FlatDirectory address

const flatDirectory = await FlatDirectory.create({
    rpc: rpc,
    privateKey: privateKey,
    address: address,
});
```


### 2.2: Upload Files To FlatDirectory

In this section, you will upload some files into the `FlatDirectory` that you just created.

You can use `Buffer` to upload files.
```js
const request = {
    key: "test.txt",
    content: Buffer.from("big data"),
    type: 2, // 1 for calldata and 2 for blob
    callback: callback
}
await flatDirectory.upload(request);
```

You can also upload `File` objects. This applies to both browser and Node.js environments.

Browser
```js
// <input id='fileToUpload' />
const file = document.getElementById('fileToUpload').files[0];

const request = {
    key: "test.txt",
    content: file,
    type: 2,
    callback: callback
}
await flatDirectory.upload(request);
```

Node.js
```js
const {NodeFile} = require("ethstorage-sdk/file");
const file = new NodeFile("/usr/download/test.jpg");

const request = {
    key: "test.jpg",
    content: file,
    type: 2,
    callback: callback
}
await flatDirectory.upload(request);
```

```bash
const callback = {
    onProgress: function (progress, count, isChange) {
       ...
    },
    onFail: function (err) {
        ...
    },
    onFinish: function (totalUploadChunks, totalUploadSize, totalStorageCost) {
        ...
    }
};
```

### 2.3: Download Files From FlatDirectory

In this section, you will download files from the ethstorage network.

```bash
// Note: To download files, you need to specify the `ethStorageRpc`.
const ethStorageRpc = "https://[ethstorage.rpc].io";
const flatDirectory = await FlatDirectory.create({
    rpc: rpc,
    ethStorageRpc: ethStorageRpc,
    privateKey: privateKey,
    address: address,
});

const key = "test.txt";
await flatDirectory.download(key, {
    onProgress: function (progress, count, chunk) {
        ...
    },
    onFail: function (error) {
        ...
    },
    onFinish: function () {
        ...
    }
});
```



## Step 3: Manage Blobs

### 3.1 Create EthStorage

In this section, you can create an `EthStorage` instance to interact directly with EthStorage for managing blobs.

```js
const { EthStorage } = require("ethstorage-sdk");

const rpc = "https://rpc.beta.testnet.l2.quarkchain.io:8545";
// For Sepolia:
// const rpc = "https://rpc.sepolia.org";
const ethStorageRpc = "https://[ethstorage.rpc].io";
const privateKey = "0x...";

const ethStorage = await EthStorage.create({
    rpc: rpc,
    ethStorageRpc: ethStorageRpc,
    privateKey: privateKey,
});
```

### 3.2 Write Blob

Write blob data to the EthStorage network.

```js
const key = "test.txt";
const data = Buffer.from("test data");
await ethStorage.write(key, data);
```

### 3.3 Read Blob

Read the written data from the EthStorage network.

```js
const key = "test.txt";
const data = await ethStorage.read(key);
```
