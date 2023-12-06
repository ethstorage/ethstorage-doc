---
description: >-
  An overview of EthStorage, including the challenges it addresses and the
  objectives it aims to accomplish.
---

# Introduction

## What is EthStorage

EthStorage is a storage-specific L2 network that reuses Ethereum security to extend Ethereum storage capabilities via data availability technology.

Specifically, EthStorage employs data sharding to achieve decentralized storage for large dynamic datasets and to support key-value (KV) storage applications on an EVM-compatible blockchain.

## Motivation

The main motivation behind EthStorage is to enhance Ethereum's storage capabilities by leveraging its security properties.

Firstly, EIP-4844/Danksharding data is only available temporarily, meaning it will be discarded in a few weeks. EthStorage will enable users to extend the availability time of DA blobs in a fully decentralized manner if they choose to do so.

Additionally, most existing L2 solutions such as rollups primarily focus on scaling Ethereum's computational power, i.e., increasing TPS. However, the demand for securely storing large amounts of data on the Ethereum mainnet has surged, especially due to the popularity of dApps like NFTs and DeFi. For instance, the storage demand is pronounced in on-chain NFTs, where not only the token of an NFT contract is owned by users, but also the on-chain images. Storing these images with a third party introduces additional trust, which can be easily compromised.

Another demand arises from the front end of dApps, mainly hosted by centralized servers (with DNS). This setup makes the websites susceptible to censorship and other issues such as DNS hijacking, website hacking, or server crashes, as evidenced by incidents like Tornado Cash.

## Goals

By leveraging Ethereum mainnet security, we can address the aforementioned problems promptly. However, if all data is stored on-chain, it will lead to significantly high costs.

Through the utilization of L2 and data availability technologies, we have the potential to achieve an Ethereum storage scaling solution with specific objectives:

* Increase the storage capacity to petabytes or more, considering that each node possesses several terabytes of storage.
* Reduce the storage expenses to 1/100th or 1/1000th of the cost [compared to SSTORE](broken-reference).
* Incorporate similar [KV CRUD semantics](storage-contract/store-data-by-using-blobs.md#the-semantics-of-the-kv-store) as SSTORE, with certain [limitations](storage-contract/get-data.md#limitations-on-get) to be considered.
* Reuse the Ethereum mainnet's security on block re-organization, storage cost settlement, and censorship resistance.

## Applications

Here are a few examples of the applications of EthStorage:

* Providing a long-term decentralized storage solution for various Rollups, such as Optimistic Rollups and ZK Rollups.
* Enabling fully decentralized frontends for dynamic websites, distinguishing from FILECOIN/AR which can only support static ones.
* Offering native storage for NFTs, allowing them to fully belong to their owners.
