---
description: Alloc8 Smart Accounts
---

# Modular Smart Accounts

Alloc8’s smart accounts are built on top of the **ERC-4337 account abstraction standard**, extended with **modularity**. At the core is the `Modular4337Account` — a **highly composable**, **upgrade-friendly**, and **developer-extensible** smart wallet contract.

At the center is the **Modular4337Account**: a smart wallet that supports plug-and-play modules for custom logic, without ever changing the core contract. Using a **hook-based architecture**, developers can add validation rules, execution logic, policy enforcement, fee routing, and more — safely and flexibly.

***

#### Key Capabilities

* **Modular Hook Design**\
  Add or remove functionality with `installModule()` and `removeModule()`. Only use what you need.
* **Validation Hooks**\
  Enforce arbitrary preconditions during validateUserOp() — such as session keys, time locks, multi-sig auth, or custom signature schemes.
* **Execution Hooks**\
  Modify or extend transactions on-chain — e.g., allowlists, fee rebates, or automatic forwarding.
* **ERC-1271 Signature Compatibility**\
  Works with dApps expecting standard ECDSA signatures.
* **Native ERC-4337 Integration**\
  Compatible with the EntryPoint contract and bundler flows for gasless UX and programmable userOps.
* **Extensible by Design**\
  Any module that follows the `IModule` interface can be plugged in, enabling safe upgrades and new features.

***

#### Why Use Modular4337Account?

Unlike fixed-function smart wallets, `Modular4337Account` it lets you compose wallet features like plug-ins in a software framework:

* You don’t need to deploy new contracts to add new capabilities/features to the wallet.
* You can progressively add features like **social recovery**, **role-based access**, **ERC20 spending caps**, or **session delegation**.
* Modules are auditable, reusable, and independently testable.

**This is the programmable core of Alloc8 wallets -** enabling AI-driven, policy-based execution and advanced DeFi strategies, while keeping full non-custodial control in the user’s hands.
