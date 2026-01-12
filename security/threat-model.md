# Threat Model

This model highlights realistic failure/attack scenarios and the intended mitigations.

#### A. Session agent key compromise

**Scenario:** An attacker gains control of the agent signer.

**Mitigations**

* Recipient pinning prevents arbitrary destination changes
* Allowlists prevent calling arbitrary contracts/tokens
* Rolling budgets cap cumulative loss (reverts beyond cap)

**Operator response**

* Pause agent
* Rotate/revoke session key
* Withdraw/reduce exposure until confident

#### B. Oracle failure or manipulation

**Scenario:** Oracle providers revert, are misconfigured, or return incorrect values.

**Mitigations**

* Fallback provider logic (main → backup)
* Revert when both fail, or the asset is unsupported

**Operator response**

* Pause agent
* Withdraw funds from the pool to the user's Smart Wallet if needed
* Do not widen slippage to force execution during Oracle instability

#### C. External protocol risk (AMM/router)

**Scenario:** Underlying AMM contracts or routers behave unexpectedly or are attacked.

**Mitigations**

* Restrict targets/selectors to known integrations
* Enforce min-out and deviation budgets

**Operator response**

* Pause agent
* Withdraw funds from the pool to the user's Smart Wallet if needed
* Follow incident communications

#### D. Admin key risk/configuration risk

**Scenario:** Admin-controlled parameters are misconfigured or compromised.

**Mitigations**

* Minimize admin surface area
* Operational controls (multisig, timelocks, reviews) — document explicitly

**Operator response**

* If an incident is suspected, pause the agent & contracts
* Verify official communications and on-chain changes
