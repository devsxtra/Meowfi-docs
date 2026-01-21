# Autonomous LP

Alloc8 automates **concentrated liquidity (CL)** so you don’t need to manually manage ranges and rebalances. CL can offer higher fee potential, but if the price moves **out of range**, your position may earn **no fees** until it is moved back in range.

#### What Alloc8 automates

**1) Range selection (ticks)**\
The **Agent** calculates the **lower and upper ticks** using pool conditions (price, TWAP, liquidity, volatility) and considers risk/gas constraints.

**2) Rebalancing (move to a new range)**\
When needed, the system can rebalance by adjusting your LP NFT position (mint/increase/decrease/collect/burn) through the **Position Manager**.

**3) Autocompound (scheduled)**\
Autocompound runs **every Sunday at 7:00 PM UTC**.

***

### ZAPs (enter/exit from any token)

Alloc8 supports **ZAPs**, which lets you **enter or exit a position using a single token**, instead of supplying (or receiving) both pool tokens.

How it works (user view):

* You choose the token you want to deposit/withdraw.
* The ZAP route performs the needed swaps and liquidity steps automatically.

**Important:** Zap outputs are **estimates**, because swaps execute at live market prices - final amounts can vary slightly.

***

### Slippage, price impact, and safety checks

Automation still needs guardrails. Alloc8 enforces on-chain policy checks via **GSPM**, including:

* **recipient pinning** (proceeds go only to your account/owner path),
* **per-transaction slippage bounds**,
* **rolling-window slippage budgets** (caps cumulative loss),
* **token/agent allowlists**, and
* pool ↔ oracle deviation limits.

Plain-language definitions:

* **Slippage**: price changes between quote and execution; if it moves too much, execution can fail and retry later.
* **Price impact**: your trade size moves the pool price; a high impact can cause a revert.

***

### Dust (small leftover tokens)

During rebalances or Zap actions, small leftover amounts (“dust”) can happen due to rounding and swap execution. Dust is **not intended to be lost -** it typically remains as small token balances in your account flow and is returned back to your smart account.&#x20;

You can either utilise the Dust by depositing it back to existing position, or open a new position or withdraw to your EOA wallet.

Alloc8’s Position Manager includes **refunds for unused tokens** during mint/increase to prevent overpayment (a common source of dust handling).

***

### Fees and gasless execution (what users should expect)

* Alloc8 charges a **10% performance fee** on **realized yield**, not on your deposit.
* This fee is collected during **rebalance** or **autocompound** events (Sunday 7:00 PM UTC).
* Rebalances/autocompounds are **gasless for users**: the protocol pays gas and subsidizes it from the performance fee.
* Rebalances typically trigger when **earned fees/yield > gas cost**. If gas is very high, smaller positions may rebalance less often and can show **out of range**. Recommended size: **$500+**.

***

### Critical risks (don’t skip)

Alloc8 automation does not remove DeFi risks, including:

* **Impermanent loss (IL)**
* **Range risk** (out-of-range positions may stop earning fees)
* **Sudden volatility spikes** (rebalances may be delayed or skipped)
* **Smart contract risk** (Alloc8 + AMMs/routers + tokens)

Also note: **rebalancing can realize IL**, because it may involve swapping between tokens to restore target ratios.
