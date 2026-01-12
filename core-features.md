---
description: Why use Alloc8?
---

# Core Features

Alloc8 combines programmable smart accounts, policy-based execution, and MEOW Agents to automate liquidity management.

#### Smart Accounts (modular ERC-4337)

* Modular “plug-in” design: add/remove modules to introduce new logic without changing the account core.
* Validation hooks for session keys and custom authorization.
* Execution hooks to enforce allowlists, recipients, value limits, and other constraints.
* ERC-1271 compatibility for contracts that expect signature validation.

#### Policy-Based Execution (GSPM)

GSPM enforces:

* the bound session agent (authorized signer)
* allowlists (agents and tokens)
* recipient pinning (where proceeds can go)
* per-transaction and rolling-window slippage budgets
* pool↔oracle deviation constraints
* tick-ratio sanity checks for liquidity operations

#### DeFi Agents (strategy automation)

Agents:

* analyze pool state and volatility
* compute upper/lower ticks for concentrated liquidity
* decide when rebalancing is appropriate (including skipping unstable periods)



Alloc8’s model is: autonomous execution gated by explicit, on-chain policy.
