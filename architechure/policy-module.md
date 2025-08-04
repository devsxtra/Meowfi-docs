---
description: ERC-6900 compatible module
---

# Policy Module

The `GlobalSessionPolicyModule` is an ERC-6900-compliant validation and execution hook designed to enforce **session-based permissions**, **contract selector access controls**, and **spender restrictions** for smart accounts.

This module provides a declarative security layer for smart accounts deployed via `Modular4337Account`. It can be used to authorize a **dedicated session signer** (e.g. mobile device, delegated key) and restrict operations like `approve()` calls and unsafe contract interactions.

**Features**

* **Session Agent Binding**\
  Each smart account can assign an `agent` address (i.e. a session signer) that is authorized to sign user operations. This can be updated in bulk using the factory or owner tools.
* **Spender Restrictions**\
  Limits the set of approved addresses that the smart account can call `approve()` on, preventing accidental or malicious ERC20 approval flows.
* **Function Selector Restrictions**\
  The module can whitelist specific function selectors on specific target contracts — e.g., allow calling only `deposit()` on a vault, but not `approve()` or `transfer()`.
* **Modular Installation**\
  Fully compatible with ERC-6900 modular architecture. When installed via `installModule()`, the session agent is bound to the account.
* **Full Hook Support**\
  Implements:
  * `preUserOpValidationHook` for signature validation
  * `preRuntimeValidationHook` for runtime selector checks
  * `preExecutionHook` for spender analysis
  * `postExecutionHook` (disabled by default)

**⚙️ Usage Example**

```solidity
solidityCopyEditGlobalSessionPolicyModule module = new GlobalSessionPolicyModule();
alloc8Account.installModule(address(module));

// Set an agent, spender allowlist, and selector allowlist
module.bulkUpdatePolices(
    sessionKey,
    [erc20Token],
    [vault],
    [IERC20.approve.selector],
    true
);
```

**🔄 Admin Functions**

| Function                              | Description                                            |
| ------------------------------------- | ------------------------------------------------------ |
| `bulkUpdatePolices(...)`              | Configure agent, spender allowlist, selector allowlist |
| `bulkUpdateAgentPolicies(accounts[])` | Mass-assign session keys to wallets                    |
| `onInstall(account)`                  | Hook to set agent on install                           |
| `onUninstall(account)`                | Cleans up agent mapping                                |

> ⚠️ Note: This module **does not fallback** to the owner’s signature. If a session key is invalid or missing, the operation will fail. It is explicitly designed for permissioned agent flows.
