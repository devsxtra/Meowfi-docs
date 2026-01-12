# Architecture Overview

Alloc8’s architecture connects modular smart accounts, Agents, and policy enforcement to manage and rebalance concentrated liquidity positions.

#### High-level flow

1. **Account Setup**: EOA deploys and configures a Smart Account and installs modules.
2. **Strategy Input**: The system requests optimal ranges from the Agent.
3. **Position Management**: deposits mint LP positions via the Position Manager.
4. **Rebalancing**: agent proposes operations; GSPM validates policy constraints.
5. **Execution**: operations execute through ERC-4337 (bundler → EntryPoint).
6. **Liquidity Update**: Position Manager burns/mints positions to update ranges.

Result: non-custodial, policy-based execution for continuous LP optimization.

<figure><img src="../.gitbook/assets/Screenshot 2025-06-27 at 7.27.21 PM.png" alt=""><figcaption></figcaption></figure>
