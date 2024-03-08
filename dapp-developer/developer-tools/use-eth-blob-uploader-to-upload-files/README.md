# Use eth-blob-uploader to upload files

## **Introduction**

In this tutorial, we will demonstrate how to upload files using the [eth-blob-uploader ](https://github.com/ethstorage/eth-blob-uploader)tool. We assume that

* There is an image file.&#x20;

<figure><img src="broken-reference" alt=""><figcaption></figcaption></figure>

## Step 1: Install eth-blob-uploader

You can install eth-blob-uploader by the following command

<pre><code><strong>npm i -g eth-blob-uploader
</strong></code></pre>

## Step 2: Upload Files

We can upload the image 'img.jpeg' to the 'to' address by providing parameters such as rpc, private key, to, file path, etc.

```
// eth-blob-uploader <rpc> <private-key> <file-path> <to-address>
eth-blob-uploader http://...rpc.io 0x...a7 /Users/test/img.jpeg 0xaa...
```

If it is an interaction with the same contract address, you can pass in data using -d.

```
// eth-blob-uploader <rpc> <private-key> <file-path> <contract> -d [data]
eth-blob-uploader http://...rpc.io 0x...a7 /Users/test/img.jpeg 0xbb... -d 0x...11
```

You can specify the number of blobs that come with a transaction, default is 3.

```
// eth-blob-uploader <rpc> <private-key> <file-path> <to-address> -c [blob-counts]
eth-blob-uploader https://...rpc.io 0x...a7 /Users/test/img.jpeg 0xaa... -c 6
```

You can also override the pending transaction by passing in parameters such as nonce and gas.

```
// eth-blob-uploader <rpc> <private-key> <file-path> <to-address> -n [nonce] -g [gas] -b [blob-gas]
eth-blob-uploader http://...rpc.io 0x...a7 /Users/test/img.jpeg 0xaa... -n 1 -g 1000000000 -b 2000000000
```

