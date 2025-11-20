# Use ethstorage-sdk to Upload and Download Files

## Introduction

In this tutorial, we will demonstrate how to upload and download files using the [**ethstorage-sdk**](https://github.com/ethstorage/ethstorage-sdk) tool.

The `ethstorage-sdk` provides APIs for file upload, download, and management.

In the following code examples, Sepolia is used by default.
You can switch to other chains by specifying a different RPC endpoint, such as "https://rpc.delta.testnet.l2.quarkchain.io:8545" for the [QuarkChain L2](https://quarkchain.io) testnet.

## Step 1: Install ethstorage-sdk

You can install `ethstorage-sdk` by the following command:

```bash
 npm i ethstorage-sdk
```

## Step 2: Manage Files

### 2.1 Create FlatDirectory

In this section, you will create a `FlatDirectory` contract for managing files.

```js
const { FlatDirectory } = require("ethstorage-sdk");

const rpc = "https://rpc.sepolia.org";
// For QuarkChain L2 testnet:
// const rpc = "https://rpc.delta.testnet.l2.quarkchain.io:8545";
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

```js
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

Standard SDK supports both uploading and downloading if created with `ethStorageRpc`.
For read-only scenarios, you can create a download-only SDK instance without `rpc` or `privateKey`.

**Note**: Data may take a few seconds to sync after upload. You can wait a few seconds before downloading to ensure successful retrieval.

```js
const ethStorageRpc = "https://rpc.testnet.ethstorage.io:9546"; // Sepolia
// For QuarkChain L2 testnet:
// const ethStorageRpc = "https://rpc.delta.testnet.l2.quarkchain.io:8545";

const key = "test.txt";

const callbackDownload = {
	onProgress: (progress, count, chunk) => {
		console.log(`Download progress: ${progress}% (${count} chunks)`);
	},
	onFail: (err) => {
		console.error("Download failed:", err);
	},
	onFinish: () => {
		console.log("Download finished (standard SDK).");
	}
};

// --- Option A: Standard SDK with ethStorageRpc ---
const flatDirectory = await FlatDirectory.create({
	rpc: rpc,
	ethStorageRpc: ethStorageRpc,
	privateKey: privateKey,
	address: address,
});

// Optional: only needed if downloading immediately after uploading
// await new Promise(resolve => setTimeout(resolve, 5000));

await flatDirectory.download(key, callbackDownload);


// --- Option B: Download-only SDK (read-only) ---
const downloadOnlySDK = await FlatDirectory.create({
	ethStorageRpc: ethStorageRpc,
	address: address,
});

await downloadOnlySDK.download(key, callbackDownload);
```

### 2.4: Close FlatDirectory SDK

After completing all uploading and downloading operations, you must **call** `close()` on your SDK instances to release
internal Workers and threads. Failing to do so may prevent Node.js from exiting normally.

```js
// Close the standard SDK instance (used for uploading and downloading)
await flatDirectory.close();
console.log("Standard SDK instance closed.");

// If you created a download-only SDK instance, make sure it is also closed
await downloadOnlySDK.close();
console.log("Download-only SDK instance closed.");
```


## Step 3: Manage Blobs

### 3.1 Create EthStorage

In this section, you can create an `EthStorage` instance to interact directly with EthStorage for managing blobs.

```js
const { EthStorage } = require("ethstorage-sdk");

const rpc = "https://rpc.sepolia.org";
const ethStorageRpc = "https://rpc.testnet.ethstorage.io:9546"; // Sepolia

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

// Optional: only needed if downloading immediately after uploading
// await new Promise(resolve => setTimeout(resolve, 5000));

const data = await ethStorage.read(key);
```

### 3.4: Close EthStorage SDK

After completing all uploading and downloading operations with `EthStorage`, you must **call** `close()` on your SDK instances
to release internal Workers and threads. Failing to do so may prevent Node.js from exiting normally.

```js
await ethStorage.close();
console.log("SDK instance closed.");
```
