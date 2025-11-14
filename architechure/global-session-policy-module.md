# Global Session Policy Module

**What this is:** An ERC-6900–style validation & execution hook designed for **Modular4337Account**.\
It follows the ERC-6900 hook pattern (install/uninstall + pre/post hooks) **using project-local interfaces** (`IValidationHookModule`, `IExecutionHookModule`) and enforces:

* Per-account **session agent** binding (authorised UserOp signer)
* Policy firewall for **vault PositionManager** flows, **ERC-20 approvals**, and **0x** routes
* **Recipient pinning** (vault/0x proceeds → account or owner)
* Global allow-lists for **agents** and **tokens**
* Rolling-window **slippage budgets** (liquidity, swaps) and **pool↔oracle deviation** budget
* **Tick-ratio** sanity checks with **single-sided** liquidity allowances

> **Compatibility note:** This module is **not a drop-in ERC-6900 reference interface**.\
> It is designed for accounts that call these hooks exactly as Modular4337Account does.\
> If a different ERC-6900 account expects the canonical interfaces/selectors or invokes hooks outside EntryPoint execution, you’ll need a thin adapter (or to align interfaces) and possibly relax the `sender == entryPoint` guard.

### Hook behaviour (summary)

* `preUserOpValidationHook` — verifies UserOp signature against the account’s **agent**.
* `preRuntimeValidationHook` — **EntryPoint-only**, forbids ETH value, blocks self-target, checks agent allow-list.
* `preExecutionHook` — enforces:
  * **Vault PM** (`collect/decrease/burn/mint/increase`): recipient checks; per-tx slippage bound (`maxSlippageCheck`); user **tick-ratio** deviation ≤ `allowedTicRatioDeviationBps`; pool↔oracle **skew** charged to deviation budget.
  * **ERC20 `approve`**: token must be `isTokenValid` Spender is a registered **vault** or **0x allowance holder**.
  * **0x AllowanceHolder → Aggregator Settler**: inner target/selector **pinned**; src/dst tokens valid; recipient pinned; **oracle-based min-out** enforced; loss charged to **swap budget**.
* `postExecutionHook` — measures realised slippage for vault mint/increase and charges the per-(account, vault) budget; can revert with `HighValueLoss()` on window cap breach.

### Budgets & tuning

* Globals (owner-settable):\
  `slippageBudgetBps`, `swapSlippageBudgetBps`, `deviationSlippageBudgetBps`,\
  `allowedTicRatioDeviationBps`, `maxSlippageCheck`, `slipWindow` (1h default).
* Per-account overrides: `updateSlippageThrottle(...)` (+ `keepSlipState`).

### Security considerations

* Hooks are **no-ops outside EntryPoint** by design (`sender == entryPoint` guard); portability may require adjusting this.
* Oracle freshness/liveness directly impacts slippage/deviation checks.
* Expanding token/agent allow-lists or relaxing budgets increases the attack surface.

### Admin / Owner API

All functions below are **`onlyOwner`**:

* `bulkUpdateAgents(address[] agents, bool allowed)`\
  Add/remove **agents** from the global allow-list (reverts on zero address). Controls who may be bound as a session agent.
* `bulkUpdateValidTokens(address[] tokens, bool valid)`\
  Add/remove **ERC-20 tokens** from the global valid-token list used by approvals and 0x flows.
* `setMaxSlippageCheck(uint256 _maxSlippageCheck)`\
  Sets per-transaction min-out threshold in **basis points** (0 < value ≤ `PERCENTAGE_FACTOR`).\
  Used in vault `mint/increase` and 0x min-out comparisons.
* `setSlippageBudgetBps(uint32 _slippageBudgetBps)`\
  Sets the **liquidity** cumulative slippage budget per window (must be > 0).
* `setSwapSlippageBudgetBps(uint32 _swapSlippageBudgetBps)`\
  Sets the **swap** cumulative slippage budget per window for 0x flows (must be > 0).
* `setDeviationSlippageBudgetBps(uint32 _deviationSlippageBudgetBps)`\
  Sets the cumulative **pool↔oracle deviation** budget per window (must be > 0).
* `setAllowedTicRatioDeviationBps(uint32 _allowedTicRatioDeviationBps)`\
  Max tolerated **user tick-ratio** deviation from expected (0 < value ≤ `PERCENTAGE_FACTOR`).
* `setPriceOracle(address _priceOracle)`\
  Updates the **oracle** used for USD valuations and skew checks (non-zero address).

> ⚠️ Note: This module **does not fallback** to the owner’s signature. If a session key is invalid or missing, the operation will fail. It is explicitly designed for permissioned agent flows.
