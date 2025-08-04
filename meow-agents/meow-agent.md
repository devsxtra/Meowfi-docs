---
description: Agent to calculate the upper and lower tick to place the liquidity in.
---

# Meow Agent

### Dynamic Tick Placement & Rebalancing Intelligence

**Meow Agent** is an autonomous strategy module that intelligently manages concentrated liquidity positions by:

* **Analysing pool conditions and volatility**
* **Computing optimal tick ranges for entry**
* **Deciding when to rebalance or rotate capital**

Designed for use with **Camelot V3**, **Uniswap V3**, and other Algebra-based AMMs, this agent enables market makers and DAOs to **maximise capital efficiency** while maintaining tight control over risk and gas costs.

***

#### What It Does

**Dynamic Tick Discovery**\
Continuously evaluates pool state (price, TWAP, liquidity depth, volatility) and calculates the optimal lower/upper ticks to deploy liquidity for a given capital allocation.

**Rebalance Signalling**\
Monitors position drift, impermanent loss, and pool movement. If a position has moved outside of its optimal range or becomes inefficient, the agent emits a **rebalance trigger** — allowing automatic or manual repositioning.

**Volatility-Aware Adjustments**\
Uses TWAP deviation, tick skew, and recent volatility bands to **avoid deploying during unstable market phases**.

**Capital-Aware Deployment**\
Takes into account slippage, token ratio, and min/max capital bounds to place liquidity where it earns the highest fees relative to risk.

***

#### Use Cases

* **Protocol-Owned Liquidity (POL)** — Manage DAO-owned liquidity adaptively and securely.
* **Automated LP Bots** — Plug into vaults or keepers to trigger on-chain `mint` or `burn`.
* **Retail Vaults** — Optimise liquidity deployment for passive users.
* **Strategy Builders** — Use as a plug-in module for more complex market-making strategies.

***

#### Sample Workflow

1. **Price Monitoring**\
   Agent fetches pool state: current tick, sqrtPrice, liquidity distribution.
2. **Tick Computation**\
   Using volatility, TWAP, and strategy parameters, it selects optimal `lowerTick` and `upperTick`.
3. **Rebalance Evaluation**
   * Checks if the existing position is out of range(here, the range is a little smaller than the overall range of the position, as we would want to rebalance if the position reaches near the border)
   * Assesses fee performance vs capital at risk
   * Signals rebalance if deviation exceeds the threshold
4. **Execution Path (Optional)**\
   Meow Agent can be paired with an executor that:
   * Burns old position
   * Mints new liquidity
   * Routes fees to treasury

***

#### Security-First Design

* Compatible with TWAP-based volatility guards (like `onlyCalmPeriods`)
* Supports capital bounds, slippage checks, and time-locks
* Designed to avoid rebalances during oracle lags or flash crashes
