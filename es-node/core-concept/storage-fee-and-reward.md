---
description: An introduction to the economic model of EthStorage.
---

# Storage Fee and Reward

To store data in the proposed decentralized KV store, users must pay storage costs to the nodes hosting shard data in order to ensure sufficient replication. How much upfront fee should we charge per KV store/blob so that the protocol can cover the estimated storage fees for 30-50 physical replicas over time?

## A discounted payment flow model

We implement a storage rental model using a discounted cash flow algorithm. Assuming storage costs (e.g. per TB per year) decrease over time relative to ETH, the upfront payment at genesis for a key store is `x = c/(1 − d)`, where `d` is the discount rate relative to ETH and `c` is a constant to adjust the base payment.

For example, suppose 0.1 ETH may cover 1TB of storage costs for the first year, with costs discounted to 95% in the second year. Then in the second year, we would only need `0.1 * 0.95 = 0.095 ETH`, and in the third year only `0.1 * 0.95 * 0.95 = 0.09025 ETH`. The upfront fee for perpetual 1TB storage would be `0.1 / (1 - 0.95) = 2 ETH`. This is a reasonable assumption given ETH's deflationary nature.

If a user stores the same key with a replaced value, we will not re-charge the storage rental cost (though normal gas fees still apply). Payments can be refunded when the KV entry is removed, based on the duration of time that the user has rented the storage.

## Storage cost estimation

To incentivize storing a reasonable number of physical replicas, we need to estimate the upfront storage fee for each blob and the payment schedule based on the discounted storage payment/rental model.

Assuming a 2TB SSD with a 5-year lifespan and maximum power consumption of 7.2 W/h (from Samsung 980 Pro spec) costs $200, and the electric price is $0.068 kW-h, the ETH price is $2000, then the storage cost per blob for the first year is approximately:

```
($200/5 + $0.068 × (7.2/1000) × 24 × 365) / 2000 × 128KB/2TB ≈ 1.5 Gwei
```

Suppose we are supporting 50 physical replicas, with a 50% profit margin for data providers, and a yearly discounted rate of 95% for ETH/TB. In this case, the storage contract will require users to pay an upfront storage fee per blob (only the first time calling put() with a non-existing key):

```
1.5 × 1/0.5 × 50 / (1-0.95) = 3000 Gwei
```

This amount equates to $0.006 at an ETH price of $2000.

As a comparison, using SSTORE (storing 32 bytes with 20000 gas) with an assumed gas price of 10 Gwei, storing 128KB data on-chain will cost

```
(128KB/32) × 20000 × 10 = 819200000 Gwei = 0.8192 ETH
```

This implies that the cost-saving ratio is about 270000 (819200000/3000).

## Fee distributor

Verifying the storage of individual blobs from each user is highly inefficient, primarily because the storage fee may not sufficiently cover the verification gas fee. As a solution, batch verification of the proof of storage for blobs from all users is necessary to significantly reduce the amortized verification cost.

Specifically, the smart contract consolidates payments from all users beginning from the last proof submission time and evenly allocates them to the storage providers who have proven to store each physical replica of the blobs.

On the es-node side, the process of PoRA involves continuous sampling of the blobs to demonstrate their eligibility for collecting the storage fee. However, the expected interval between submissions is much larger (e.g. a few hours) so the gas costs to submit the proof via an L1 transaction can be amortized over time. This process provides storage providers periodic rewards while balancing on-chain costs.
