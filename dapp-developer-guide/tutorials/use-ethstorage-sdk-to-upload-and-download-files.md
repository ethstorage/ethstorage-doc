
# Use ethstorage-sdk to upload and download files

## **Introduction** <a href="#introduction" id="introduction"></a>

The `ethstorage-sdk` provides APIs for file upload, download, and management, which are built on `c-kzg` and, therefore, are not compatible with a browser environment.

In this tutorial, we will demonstrate how to upload and download files using the [**ethstorage-sdk**](https://github.com/ethstorage/ethstorage-sdk) tool.

## Step 1: Install ethstorage-sdk <a href="#step-1-install-ethstorage-sdk" id="step-1-install-ethstorage-sdk"></a>

You can install [ethstorage-sdk](https://github.com/ethstorage/ethstorage-sdk) by the following command

`npm i ethstorage-sdk`

## Step 2: Upload Files <a href="#step-2-upload-files" id="step-2-upload-files"></a>

### **Node.js**

```
const {BlobUploader, EncodeBlobs} = require("ethstorage-sdk");

const upload = async (filePath) => {
    const rpc = "https://goerli.rpc.io";
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

## Step 3: Management Files <a href="#step-3-management-files" id="step-3-management-files"></a>

### **3.1 Create Flat Directory**

In this section, you will create a Flat Directory contract for managing files.

**Node.js**

```
const {EthStorage} = require("ethstorage-sdk");

const create = async () => {
    const ETH_STORAGE_ADDRESS = "0xb4B46bdAA835F8E4b4d8e208B6559cD267851051";
    const rpc = "https://goerli.rpc.io";
    const privateKey = "0x...";
 
    const ethStorage = new EthStorage(rpc, privateKey);
    await ethStorage.deployBlobDirectory(ETH_STORAGE_ADDRESS);
    // flatDirectory address: 0x37df32c7a3c30d352453dadacc838461d8629016
}
```

### **3.2: Upload Files To Flat Directory**

In this section, you will upload some files into the FlatDirectory that you just created.

**Node.js**

```
const {EthStorage} = require("ethstorage-sdk");

const upload = async (filePath) => {
    const rpc = "https://goerli.rpc.io";
    const privateKey = "0x...";
 
    const ethStorage = new EthStorage(rpc, privateKey);
    await ethStorage.upload(filePath);
    // hash: 0x753...45836
}
```

### **3.3: Download Files From Flat Directory**

In this section, you will download files from the ethstorage network.

**Node.js**

```
const {DownloadFile} = require("ethstorage-sdk");

const download = async (fileName) => {
    const flatDirectory = "0x37df32c7a3c30d352453dadacc838461d8629016";
    // Note: This should be ethstorage rpc, not eth rpc
    const rpc = "https://ethstorage.rpc.io";
 
    const data = await DownloadFile(rpc, flatDirectory, fileName);
    // You can save the data as a file
}
```

