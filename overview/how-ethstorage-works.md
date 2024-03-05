---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# How EthStorage Works

The EthStorage network comprises two primary components: a storage contract deployed on the Ethereum L1, and L2 storage nodes such as es-node, responsible for actual data storage.

This is the way how it works:

## Step 1/5

Users upload their data to an application contract, which then interacts with the EthStorage contract to store the data.

<figure><img src="https://ethstorage.io/img/work1.d595c1f9.svg" alt=""><figcaption><p>step 1/5</p></figcaption></figure>

## Step 2/5

In the EthStorage L2 network, storage providers receive notifications about data awaiting storage.

<figure><img src="https://ethstorage.io/img/work2.ee049c2c.svg" alt=""><figcaption><p>step 2/5</p></figcaption></figure>

## Step 3/5

Storage providers download data from the Ethereum data availability network.

<figure><img src="https://ethstorage.io/img/work3.89adbac4.svg" alt=""><figcaption><p>step 3/5</p></figcaption></figure>

## Step 4/5

Storage providers submit storage proof to L1, proving a substantial number of replicas in the L2 networks.

<figure><img src="https://ethstorage.io/img/work4.bb8f04dc.svg" alt=""><figcaption><p>step 4/5</p></figcaption></figure>

## Step 5/5

The EthStorage contract rewards the storage provider who successfully submits the storage proof.

<figure><img src="https://ethstorage.io/img/work5.165c0bba.svg" alt=""><figcaption><p>step 5/5</p></figcaption></figure>

