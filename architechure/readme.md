# Architecture Overview

This architecture shows how Alloc8’s smart accounts and MEOW Agent work together to manage and rebalance Uniswap V3 liquidity. AI suggests optimal ranges, policies enforce safe execution, and all actions remain non-custodial and on-chain.

* **Account Setup** – The EOA deploys and validates a modular Smart Account (Alloc8Account), installs modules, and creates a session.
* **Strategy Input** – The SessionPolicyModule requests optimal tick ranges from the MEOW Agent, which uses AI signals to return the suggested range.
* **Position Management** – The MEOW Agent deposits funds and mints a liquidity position.
* **Ongoing Rebalancing** – The MEOW Agent submits rebalance operations, validated through policy rules.
* **Execution Pipeline** – Transactions pass via the bundler and ERC-4337 EntryPoint to the Position Manager.
* **Liquidity Update** – The Position Manager interacts with Uniswap V3 to burn old positions and mint new ones, keeping liquidity in the optimal range.

**Result:** Non-custodial, policy-based AI execution that continuously optimizes LP positions on-chain.

<figure><img src="../.gitbook/assets/Screenshot 2025-06-27 at 7.27.21 PM.png" alt=""><figcaption></figcaption></figure>
