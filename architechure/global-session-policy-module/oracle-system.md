# Oracle System

***

### Core Components

#### 1. **PriceOracle Contract**

The PriceOracle contract aggregates pricing information from multiple external oracle sources. It supports configuration of a **primary provider** and an optional **backup provider** per asset, enabling graceful degradation when a source fails.

**Responsibilities**

* Maintain mapping of assets → provider data (main + backup)
* Expose a single canonical function for price lookup in USD with 18 decimals
* Provide fallback logic when primary feeds fail
* Emit update events for off‑chain monitoring

**Key Properties**

* **Deterministic failover:** if primary reverts, backup is queried
* **Configurable per asset:** different assets can have different providers
* **Upgradeable configuration:** only `owner` (oracle admin) can update
* **Gas‑optimized sequential lookup**

***

#### 2. **Price Providers (External Oracle Interfaces)**

PriceOracle depends on external contracts that implement:

```solidity
function getAssetPrice(address asset) external view returns (uint256);
```

These contracts abstract different oracle sources such as Chainlink, Api3, etc.

**Provider Types (examples)**

| Provider Type | Example Use Case                 |
| ------------- | -------------------------------- |
| Chainlink     | Primary reliable feed for majors |
| Api3          | Secondary real‑time fast updates |



***

### Data Flow Overview

```
┌──────────────┐           ┌─────────────────────┐           ┌──────────────────────────┐
│  External     │           │   PriceOracle       │           │ Global Session Policy     │
│  Providers    │           │   (Aggregator)      │           │ Module (GSPM)            │
│ (CL / PYTH    │  prices   │                     │  prices   │                          │
│  / Redstone)  ├──────────►│ getAssetPrice()     ├──────────►│ validateUserOperation()  │
└──────────────┘           │ fallback + events    │           │ enforce slippage limits   │
                            └─────────────────────┘           └──────────────────────────┘
```

***

### Usage in Global Session Policy Module (GSPM)

The GSPM consumes USD prices for assets involved in a user operation. Prices are used for:

* **Value checks** (ensuring max allowed spend value per operation)
* **Slippage enforcement** for swaps
* **Collateral & risk checks** for positions under managed agents

#### Example: Swap Validation

```
expectedAmountOut = inputValueUsd * (1 - maxSlippageBps / 10_000)
actualAmountOut = router.call(...)
require(actualAmountOut >= expectedAmountOut)
```

PriceOracle resolves USD value per asset using:

```
_price = PriceOracle.getAssetPrice(asset)
value = amount * price / 1e18
```

***

### Security Model

#### Defense Measures

* Fallback provider resilience
* Owner‑gated configuration updates
* Reverts on unsupported assets or failing feeds
* On‑chain traceability via `PriceProvidersUpdated` events

#### Oracle Failure Scenarios

| Failure Case                | Module Outcome                 |
| --------------------------- | ------------------------------ |
| main feed revert            | fallback attempt               |
| both revert                 | transaction reverts            |
| misconfigured provider      | revert & monitoring alert      |
| stale backup but valid main | accepted                       |
| wrong decimal / scaling     | caught in testing requirements |

***

### Operational Responsibilities

| Role                   | Responsibilities                         |
| ---------------------- | ---------------------------------------- |
| Oracle Admin           | Update providers, monitor failure events |
| Observability / DevOps | Alert on backup usage spikes             |
| Agent Providers        | Consume prices to enforce policies       |

***

### Future Extensions

* Support median‑of‑N providers
* Price risk engines per strategy instead of flat slippage bps

***

### Appendix

#### Events

```
event PriceProvidersUpdated(address asset, address main, address backup);
```

#### Error Types

* `InvalidParams()`
* `UnsupportedAsset(address asset)`
* `PriceFeedNotWorking(asset, main, backup)`

***
