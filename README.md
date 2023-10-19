# Introduction

## What is EthStorage

EthStorage is a storage-specific L2 network that reuses Ethereum security to extend Ethereum storage capabilities via data availability technology.

## Motivation

The major goal of EthStorage is to extend Ethereum storage capabilities by reusing Ethereum security properties.

EIP-4844/Danksharding data is only temporarily available, i.e., the data will be discarded in a few weeks. EthStorage will allow users to optionally extend the availability time of DA BLOBs in a fully decentralized way.

Most existing L2 solutions such as rollups focus on scaling Ethereum computation power, a.k.a., higher transactions per second. Meanwhile, with the popularity of dApps such as NFT/DeFi/etc, the demand for storing a large amount of data reusing Ethereum mainnet security has grown dramatically. For example, one strong storage demand comes from on-chain NFTs, where not only the token of an NFT contract is owned by the users, but also the on-chain images belong to the users. In contrast, storing the images on a 3rd party (e.g., ipfs or centralized servers) introduces additional trust, which can be easily and frequently broken (e.g., a lot of images of old NFT projects using ipfs are now unavailable).

Another demand is the front end of dApps, which are mostly hosted by centralized servers (with DNS). This means that the websites can be easily censored (happening for Tornado Cash). Further issues include DNS hijacking, website hacking, or server crash.

By reusing Ethereum mainnet security, all the aforementioned problems can be immediately solved. However, if everything is stored on-chain, the cost will be extremely high - for example, storing 1GB data using SSTORE will cost 1GB / 32 (per SSTORE) * 20000 (gas per SSTORE) * 10e9 (gas price) / 1e18 (Gwei to ETH) * 1500 (ETH price) = $10M! The cost can be reduced to 1/3x using a contract code, but it is still far more expensive than other storage solutions (S3/FILECOIN/AR/etc).

With L2 and data availability technologies, we believe that we can achieve an Ethereum storage scaling solution with the following goals:

- Increase the capacity to PB or more assuming that each node has a few TB disks
- Reduce the storage cost to 1/100x or 1/1000x vs SSTORE
- Similar KV CRUD semantics as SSTORE (a few limitations will apply, see below)
- Reuse Ethereum mainnet security on block re-organization, storage cost settlement, and censorship-resistant

## Application

These are some examples for the applications of EthStorage among others:
- Long-term decentralized storage solution for other Rollups including Optimistic Rollups and ZK Rollups.
- Fully decentralized frontend with dynamic websites where FILECOIN/AR can only serve static ones.
- Native storage for NFTs that are fully belong to their owners.

  