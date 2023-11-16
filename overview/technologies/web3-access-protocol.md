---
description: >-
  Web3URL is an access protocol for future web3 that seeks to solve the problem
  of decentralized content location.
---

# web3:// Access Protocol

## Background

The growth of decentralized apps like DeFi and NFTs has created excitement about Web3, which offers censorship resistance and transparency compared to centralized Web2.

However, accessing Web3 requires users to connect to blockchain nodes through wallets and node service providers (NSPs), resulting in centralization which grants NSPs monopolistic data access and the power to censor content. Users could run nodes themselves but maintaining servers is difficult, and centralized services iterate faster than decentralized platforms. Ultimately, Web3's benefits are constrained by reliance on intermediaries, showing the challenges in fully decentralizing.

## Our Solution

We expect that as more and more users and developers face the current problems of Web3 (or Web2.5 more precisely?), we need a solution so that we could fully harvest the benefits of blockchain. That is why we believe the future of Web3 should be

> End-to-End Fully Trustless Decentralized Web

This means that any components in our Web2 from the user side and server side must be decentralized in Web3.

* We can still rely on TCP/IP protocol, which is already decentralized and used by the blockchain P2P network.
* We should not rely on the DNS protocol as it could still censor a translation of a domain. Instead, we could use **blockchain-based name services** such as ENS.
* We should not use the current client/webserver model, where the webserver itself is centralized. Instead, we could use a blockchain network where **the blockchain P2P network itself can serve as a webserver**!
* We do not want to change current users’ web experience, especially the HTTP protocol. This can be done by designing the blockchain network support HTTP protocol in a decentralized way (decentralized HTTP?)!

Achieving all these seem to be extremely challenging. The following graph illustrates one solution, where

1. The user installs a verified extension (like downloading geth from github), which serves as a light client to the blockchain P2P network.
2. When the user types a web3 URL (e.g., web3://xxxxx), the extension will parse the URL and translate it to a blockchain message (e.g., calling a smart contract). Then the extension will deliver the message to the P2P network and query the result. For any result returned from the network, the extension fully verified that the result is trusted.
3. The trusted result is returned to the web browser. The result will be mostly like an HTML document that may contain more web3 URLs.

## Examples

In the following, let me illustrate a couple of examples of what it looks like from the user’s perspective.

### Fully decentralized exchange

Suppose a user wants to use a fully decentralized exchange in Web3, the user can do the following steps:

1. The user types a web3 address: `web3://uniswap.eth` in a web browser
2. The light client extension finds the contract address of `uniswap.eth` in ENS (suppose the result is `0xaabbccddee…`)
3. The light client extension calls the contract `0xaabbccddee…` (without parameters)
4. The EVM in the blockchain network responds with the content of the webpage in “`<HTML> … </HTML>`” with cryptographic proofs
5. The light client verifies the result and then displays it on the web browser

### Fully decentralized social network

Suppose a user receives a link to a tweet, the user can read the tweet as follows.

1. The user types a web3 address: `web3://twitter.eth/view/1125236289743`
2. The light client extension finds the contract address of `twitter.eth` in ENS (suppose the result is `0x1122334455…`)
3. The light client extension calls the contract `0x1122334455…` with `MethodID=”view”` and `Argument=uint256(1125236289743)`
4. The EVM in the blockchain network responds with the content of the webpage in “`<HTML> … </HTML>`” with cryptographic proofs
5. The light client verifies the result and then displays it on the web browser.
