# Overview

Alloc8 is an agentic asset allocation protocol that lets you programmatically delegate execution using smart accounts. Alloc8 is built for Liquidity Providers (LPs) and DAOs that want automation without giving up custody.

#### What “non-custodial” means in Alloc8

Alloc8 is designed so that:

* Funds live in your **Smart Account** (a modular ERC-4337 smart wallet).
* An **agent** can only execute actions that your **policy** allows.
* If the policy checks fail, execution fails.

In other words, the agent can automate, but it cannot exceed the permissions you grant.

#### The mental model

Alloc8 has five moving pieces:

1. **Modular Smart Account:** Holds your funds and enforces modules (validation + execution hooks).
2. **Account Factory:** Deterministically deploys accounts and maintains on-chain registry metadata.
3. **Global Session Policy Module (GSPM):** The policy firewall: binds the session agent and validates that actions are allowed.
4. **Position Manager:** Mints/manages LP positions as NFTs (mint/increase/decrease/collect/burn) with additional controls.
5. **DeFi Agent:** Computes tick ranges and proposes rebalances based on pool conditions and volatility.

#### User lifecycle (end-to-end)

1. Create a Smart Wallet.
2. Activate your Agent (session + policy).
3. Deposit into a strategy.
4. Agent proposes rebalances; policy enforces safe execution.
5. Withdraw at any time.

#### What this documentation covers

* **Product guides** for users and operators.
* **Architecture reference** for builders and auditors.
* **Security & risk** to clarify guarantees, assumptions, and failure modes.



If you are integrating or auditing, start with **Architecture → Global Session Policy Module** and **Security → Threat Model**.

