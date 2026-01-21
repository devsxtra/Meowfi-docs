# Dust during automation

**Dust** means small leftover token amounts that can remain after rebalancing or minting because:

* swaps can’t be perfectly exact,
* rounding occurs on-chain,
* slippage changes final amounts.

Alloc8’s Position Manager includes **refunds for unused tokens** during mint/increase to avoid overpaying, which is one common source of “dust.”&#x20;

**Key point:** dust is not “lost.” It typically remains in your **Smart Account** as small token balances.
