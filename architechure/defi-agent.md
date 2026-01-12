---
description: Agent to calculate the upper and lower tick to place the liquidity in.
---

# Meow Agent

### Dynamic Tick Placement & Agentic Rebalancing

The **Meow Agent** is an autonomous strategy module that uses real-time market data to manage concentrated liquidity positions. It:

* Analyzes pool conditions and volatility
* Calculates optimal tick ranges
* Decides when to rebalance or rotate capital

Designed for **Camelot V3**, **Uniswap V3**, and other **Algebra-based AMMs**, it helps market makers and DAOs maximize capital efficiency while controlling risk and gas costs.

***

#### What It Does

**Dynamic Tick Discovery**\
Continuously scans pool state (price, TWAP, liquidity depth, volatility) to set the best lower/upper ticks for liquidity deployment.

**Rebalance Signaling**\
Tracks position drift and inefficiency. Triggers rebalance signals when liquidity moves out of its optimal range or performance declines.

**Volatility-Aware Adjustments**\
Avoids rebalancing during unstable market phases using TWAP deviation, tick skew, and volatility bands.

**Capital-Aware Deployment**\
Places liquidity where it earns the highest fees for the given risk, factoring in slippage, token ratios, and capital limits.

***

#### Use Cases

* **Protocol-Owned Liquidity (POL)** – Adaptive, policy-driven DAO liquidity management
* **Automated LP Bots** – Trigger on-chain mints/burns via vaults or keepers
* **Retail Vaults** – Optimize liquidity for passive LP users
* **Strategy Builders** – Plug-in module for advanced market-making

***

#### Sample Workflow

1. **Price Monitoring** – Fetch current tick, sqrtPrice, liquidity distribution
2. **Tick Computation** – Use volatility, TWAP, and strategy rules to select an optimal range
3. **Rebalance Evaluation** – Check range boundaries, fee performance, and capital at risk
4. **Signal Trigger** – If deviation exceeds threshold, emit a rebalance signal
5. **Execution (Optional)** – Executor burns old position, mints new liquidity, routes fees to treasury

***

#### Security-First Design

* Works with TWAP-based volatility guards (e.g., _onlyCalmPeriods_)
* Enforces capital bounds, slippage limits, and time-locks
* Avoids rebalancing during oracle lags or flash crashes

**In short:** Meow Agents keep your liquidity in the most profitable range - automatically, intelligently, and without giving up custody.
