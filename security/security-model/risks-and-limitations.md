# Risks & Limitations

Alloc8 automation does not remove fundamental DeFi risks.

#### Smart contract risk

Bugs or vulnerabilities in:

* Alloc8 contracts/modules
* Integrated AMMs/routers
* Token contracts

#### Market risks

* Impermanent loss
* Range risk (out-of-range positions)
* Fee volatility
* Sudden volatility spikes

#### Oracle risk

* Feed failure causes reverts
* Wrong prices can block or mis-evaluate min-out checks

#### Execution risk

* High gas/congestion may delay execution
* Repeated reverts can cause “missed” rebalances

#### Practical guidance

* Start small
* Keep policy settings conservative
* Pause if behavior is unclear
