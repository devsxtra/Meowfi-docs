# Security Model

Alloc8’s security posture is built around three layers:

#### 1) Smart Account layer

* Funds reside in your Smart Account.
* Actions execute through ERC-4337 entrypoint flows.

#### 2) Authorization layer (Session + Policy)

* A session agent must be bound to your account session.
* GSPM enforces:
  * allowed agents/tokens
  * recipient pinning
  * slippage and deviation constraints
  * function/target constraints for sensitive flows

#### 3) Execution layer (validated calls)

* Position management executes via Position Manager.
* Price-aware enforcement uses the Oracle System.

#### Key guarantee to internalize

An agent can only do what the policy allows.

This is not a claim that all losses are preventable; rather, it is an authorization boundary intended to prevent unauthorized fund movement and uncontrolled execution.
