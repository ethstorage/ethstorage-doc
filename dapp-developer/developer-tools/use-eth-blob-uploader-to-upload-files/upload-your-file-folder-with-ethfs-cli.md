# Upload Your File/Folder with ethfs-cli

## **Introduction**

In this tutorial, we will demonstrate how to upload files or folders using the [**ethfs**](https://github.com/ethstorage/eth-fs/)[**-cli**](https://github.com/ethstorage/ethfs-cli/) tool. We assume that

* there is a folder to be uploaded; (dist)
* there are two files in the folder. (hello.txt and img/1.jpeg)

<figure><img src="broken-reference" alt=""><figcaption></figcaption></figure>

## Step 1: Install ethfs-cli

You can install eth-fs by the following command

```
npm i -g ethfs-cli
```

## Step 2: Create a FlatDirectory Contract

Suppose we want to create a \[FlatDirectory]\([https://docs.web3url.io/advanced-topics/flatdirectory](https://docs.web3url.io/advanced-topics/flatdirectory)) can with the private key 0x112233... on the default Chain (**Ethereum Mainnet**)

```
$ ethfs-cli create -p 0x112233...
```

If we want to create it on the other chains that support EVM, we need to add the ChainId by `-c` flag, such as **Sepolia**.

```
$ ethfs-cli create -p 0x112233... -c 11155111
```

&#x20;You can also specify your own RPC for better performance by `-r` flag.

```
$ ethfs-cli create -p 0x112233... -c 7011893062 -r http://...rpc.io
```

You will get a FlatDirectory address after the transaction is confirmed.

```
$ ethfs-cli create -p 0x112233... -c 11155111
chainId = 11155111
providerUrl = https://rpc.sepolia.org
FlatDirectory Address: 0x2f7696D4284358A2E8fDb4DF772dAd60c2c8fbAd
```

## Step 3: Upload Files

In this section, you will upload the folder into the FlatDirectory that you just created.

In order to identify which chain the FlatDirectory contract is on, the short name of the chain needs to be added before the contract address. See more details about [the EIP-3770 address](https://eips.ethereum.org/EIPS/eip-3770).

```
shortName:address
```

The address created above on the **Galileo** network are spliced ​​as follows.

```
w3q-g:0x056308D76Be75817497d16281bC19a157fE7eFE6
```

Run the command to upload the file.

```
ethfs-cli deploy -f <directory|file> -a <address> -p <private-key>
```

The command to upload the contents of the "_dist"_ folder to the deployed contract is:

```
ethfs-cli deploy -f /Users/.../dist -a w3q-g:0x056308... -p 0x112233...
```

You can also set up rpc.

```
ethfs-cli deploy -f /Users/.../dist -a w3q-g:0x056308... -p 0x11... -r https://...rpc.io
```

You can specify the file upload type; "1" for calldata type, "2" for EIP-4844 blob type (requires network support for EIP-4844).

```
ethfs-cli deploy -f /Users/.../dist -a w3q-g:0x056308... -p 0x11... -t 2
```

<figure><img src="broken-reference" alt=""><figcaption></figcaption></figure>

## Step 4: Download Your File!

Now, you should be able to download the file you just uploaded.

<pre><code><strong>ethfs-cli download -a &#x3C;address> -f &#x3C;file>
</strong></code></pre>

Run the command to download the file.

```
ethfs-cli download -a devnet:0x05630... -f 1.jpeg
```

Now, your file has been saved locally.
