# DApp Developer Guide

This guide is designed to help you build your dApps with EthStorage. Here, you will discover all the essential information necessary for constructing an application utilizing EthStorage as its foundation.

## Demands for Decentralization

As people's awareness of the importance of decentralization continues to rise, the demand for on-chain storage data is gradually increasing. For instance, the storage demand is pronounced in on-chain NFTs, where not only the token of an NFT contract is owned by users, but also the on-chain images. Storing application data with a third party could save cost, but introduce additional trust that can be easily compromised. 

Another demand arises from the front end of dApps, mainly hosted by centralized servers. This setup makes the websites susceptible to censorship and other issues such as DNS hijacking, website hacking, or server crashes, as evidenced by incidents like Tornado Cash. 

## Why Build on EthStorage? 

Consider the examples mentioned above, EthStorage offers native storage for NFTs within the Ethereum ecosystem, allowing them to fully belong to their owners. EthStorage also enables fully decentralized frontends for dynamic websites, distinguishing from FileCoin/Arweave which can only support static ones. These examples raise the question: What crucial aspects should be considered when selecting a storage solution for your dApps?

### Security

Leveraging Ethereum mainnet security provides resilience against block re-organization, ensures secure storage cost settlement, and enhances censorship resistance.

### Higher Capacity and Lower Cost

Through the utilization of L2 and data availability technologies, we have the potential to achieve an Ethereum storage scaling solution with a storage capacity of petabytes or more, considering that each node possesses several terabytes of storage. 
At the same time, the storage expenses can be reduced to 1/100 or 1/1000 of the cost compared to SSTORE. Learn more detailed calculations with our online [calculator](https://docs.google.com/spreadsheets/d/1DR4I6eF6lb4pHgDrHId76OpF1gVeybXaNSEZOAdzoL4/).

### Rich Semantics (KV CRUD). 

Thanks to DA and smart contracts, EthStorage can offer full KV CRUD semantics similar to SSTORE. FileCoin/Arweave mostly works for static files, which lack efficient update/delete operations - i.e., the users have to pay twice to update existing data.

### Programmability

Smart contracts can programmatically manage storage, facilitating the easy implementation of new features such as multi-user access control and data composability.

### Zero Onboarding Cost

As EthStorage is built on top of Ethereum, storage fees are paid in ETH, allowing users to perform storage operations using ETH wallets like Metamask without the need to learn new tokens, wallets, or addresses.

### Atomicity

A classic web3 application often involves two steps: 
1. uploading the data to an external storage network 
2. storing the content hash on the chain. 
    
Through the integration of DA and EVM, EthStorage can execute both application logic and storage logic within a single transaction, simplifying operations and improving user experience, a trait commonly found in leading Web2 applications such as X and Meta.
